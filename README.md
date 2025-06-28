
<html lang="bn">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Krishi Data</title>
  <style>
    body { font-family: sans-serif; padding: 20px; background: #f0fff0; }
    h1 { color: green; }
    input, button { padding: 8px; margin: 5px; }
    .crop { background: #fff; padding: 10px; margin: 10px 0; border-left: 5px solid green; }
  </style>
</head>
<body>
  <h1>🌾 কৃষি তথ্য যোগ করুন</h1>
  <input type="text" id="name" placeholder="ফসলের নাম" />
  <input type="text" id="season" placeholder="মৌসুম" />
  <input type="number" id="price" placeholder="দাম (টাকা)" />
  <button onclick="addCrop()">তথ্য যোগ করুন</button>

  <h2>📋 ফসলের তালিকা</h2>
  <div id="cropList"></div>

  <!-- Firebase SDK -->
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/10.12.0/firebase-firestore.js"></script>

  <script>
    // আপনার Firebase config এখানে বসান
    const firebaseConfig = {
      apiKey: "AIzaSyCu_VvCJuB0uGP1ROBA8BN0NKaH6oIFTD2w",
      authDomain: "krishi-data.firebaseapp.com",
      projectId: "krishi-data",
      storageBucket: "krishi-data.appspot.com",
      messagingSenderId: "395903996011",
      appId: "1:395903996011:web:696965a98299528e29612e",
      measurementId: "G-CF3YW6L9DX"
    };

    // Initialize Firebase App
    const app = firebase.initializeApp(firebaseConfig);
    const db = firebase.firestore(app);

    // Add crop to Firestore
    function addCrop() {
      const name = document.getElementById("name").value.trim();
      const season = document.getElementById("season").value.trim();
      const price = document.getElementById("price").value.trim();

      if (!name || !season || !price) {
        alert("দয়া করে সব ফিল্ড পূরণ করুন!");
        return;
      }

      db.collection("crops").add({ name, season, price: Number(price) })
        .then(() => {
          alert("✅ ফসল যোগ হয়েছে!");
          document.getElementById("name").value = "";
          document.getElementById("season").value = "";
          document.getElementById("price").value = "";
        })
        .catch(err => {
          alert("ত্রুটি হয়েছে: " + err.message);
        });
    }

    // Show crops in real-time
    db.collection("crops").onSnapshot(snapshot => {
      const list = document.getElementById("cropList");
      list.innerHTML = "";
      snapshot.forEach(doc => {
        const crop = doc.data();
        list.innerHTML += `<div class="crop">
          <strong>${crop.name}</strong><br/>
          মৌসুম: ${crop.season} <br/>
          দাম: ${crop.price} টাকা
        </div>`;
      });
    });
  </script>
</body>
</html>
