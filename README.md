<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>ESP32 Weight Display</title>
  <style>
    html, body {
      font-family: Arial, sans-serif;
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      height: 100%;
      margin: 0;
      background-color: #f0f0f0;
    }
    .container {
      width: 90%;
      max-width: 600px;
      background: #fff;
      padding: 20px;
      border-radius: 8px;
      box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
      text-align: center;
    }
    h1 {
      color: #333;
      font-size: 2.5em;
      margin-bottom: 20px;
    }
    p {
      font-size: 1.5em;
      margin: 10px 0;
    }
    input {
      padding: 15px;
      font-size: 1.2em;
      border: 1px solid #ccc;
      border-radius: 4px;
      margin-top: 10px;
      width: 80%;
      max-width: 300px;
    }
    button {
      padding: 15px 30px;
      margin: 20px 10px;
      font-size: 1.2em;
      color: #fff;
      background-color: #007BFF;
      border: none;
      border-radius: 4px;
      cursor: pointer;
    }
    button:hover {
      background-color: #0056b3;
    }
  </style>
  <!-- Firebase App (the core Firebase SDK) is always required and must be listed first -->
  <script src="https://www.gstatic.com/firebasejs/9.8.0/firebase-app.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.8.0/firebase-analytics.js"></script>
  <script src="https://www.gstatic.com/firebasejs/9.8.0/firebase-database.js"></script>
</head>
<body>
  <div class='container'>
    <h1>Weight Display</h1>
    <p id='weight'>Weight: </p>
    <input type='number' id='unitWeight' placeholder='Enter unit weight (kg)' step='0.001'>
    <p id='units'>Number of Units: </p>
    <button onclick='tareScale()'>Tare Scale</button>
  </div>

  <script>
    // Import the functions you need from the SDKs you need
    import { initializeApp } from "firebase/app";
    import { getAnalytics } from "firebase/analytics";
    import { getDatabase, ref, onValue, set } from "firebase/database";

    // Your web app's Firebase configuration
    const firebaseConfig = { 
  apiKey: "AIzaSyCENwRUTdGb7oa2ymsSGs2TDvN0z-aQIvA",
  authDomain: "esp32-c72b0.firebaseapp.com",
  databaseURL: "https://esp32-c72b0-default-rtdb.firebaseio.com",
  projectId: "esp32-c72b0",
  storageBucket: "esp32-c72b0.appspot.com",
  messagingSenderId: "1035271337480",
  appId: "1:1035271337480:web:4b4b43b6f518f1adfa83bd",
  measurementId: "G-ERER2ESDC3"
   };

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const analytics = getAnalytics(app);
    const database = getDatabase(app);

    // Reference your database path
    const weightRef = ref(database, 'sensor/weight');
    const tareRef = ref(database, 'sensor/tare');

    // Get data from Firebase
    onValue(weightRef, (snapshot) => {
      const weight = snapshot.val();
      document.getElementById('weight').innerText = `Weight: ${weight} kg`;

      const unitWeight = parseFloat(document.getElementById('unitWeight').value);
      if (unitWeight > 0) {
        const units = weight / unitWeight;
        document.getElementById('units').innerText = `Number of Units: ${units.toFixed(2)}`;
      }
    });

    // Function to tare the scale
    function tareScale() {
      set(tareRef, true).then(() => {
        alert('Scale tared successfully');
      }).catch((error) => {
        alert('Error taring scale: ' + error.message);
      });
    }
  </script>
</body>
</html>
