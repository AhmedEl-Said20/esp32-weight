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
  <script type="module">
    // Import the functions you need from the SDKs you need
    import { initializeApp } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-app.js";
    import { getDatabase, ref, onValue, set } from "https://www.gstatic.com/firebasejs/9.6.1/firebase-database.js";

    // Your Firebase configuration
    const firebaseConfig = {
        apiKey: "AIzaSyA-qmbXDPidGFpY08xXJfpMI_cdnMaMIqA",
  authDomain: "weight-601c9.firebaseapp.com",
  databaseURL: "https://weight-601c9-default-rtdb.firebaseio.com",
  projectId: "weight-601c9",
  storageBucket: "weight-601c9.appspot.com",
  messagingSenderId: "177364828227",
  appId: "1:177364828227:web:31d795b7a0a4a98b9c94f6",
  measurementId: "G-4NJ8Q3BPGD"
    };

    // Initialize Firebase
    const app = initializeApp(firebaseConfig);
    const database = getDatabase(app);

    // Function to fetch weight data from Firebase
    function fetchWeight() {
        const weightRef = ref(database, 'weight');
        onValue(weightRef, (snapshot) => {
            const weight = snapshot.val();
            document.getElementById('weight').innerText = Weight:${weight} kg ;

            // Calculate number of units
            const unitWeight = parseFloat(document.getElementById('unitWeight').value);
            if (!isNaN(unitWeight) && unitWeight > 0) {
                const numberOfUnits = Math.floor(weight / unitWeight);
                document.getElementById('units').innerText = Number of Units: ${numberOfUnits};
            } else {
                document.getElementById('units').innerText = Number of Units: 0;
            }
        });
    }

    // Function to tare the scale
    async function tareScale() {
        try {
            await fetch('/tare');
            alert('Scale tared successfully');
            // Update the weight display to 0
            set(ref(database, 'weight'), 0);
            document.getElementById('weight').innerText = 'Weight: 0 kg';
            document.getElementById('units').innerText = Number of Units: 0;
        } catch (error) {
            console.error('Error taring the scale:', error);
            alert('Failed to tare the scale');
        }
    }

    // Call the function to fetch weight data
    fetchWeight();

    // Update units calculation when unit weight input changes
    document.getElementById('unitWeight').addEventListener('input', fetchWeight);
  </script>
</head>
<body>
  <div class="container">
    <h1>Weight Display</h1>
    <p id="weight">Weight: </p>
    <input type="number" id="unitWeight" placeholder="Enter unit weight (kg)" step="0.001">
    <p id="units">Number of Units: </p>
    <button onclick="tareScale()">Tare Scale</button>
  </div>
</body>
</html>
