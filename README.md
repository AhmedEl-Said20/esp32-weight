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
    // Your web app's Firebase configuration
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
    firebase.initializeApp(firebaseConfig);
    const database = firebase.database();

    // Reference your database path
    const weightRef = database.ref('sensor/weight');
    const tareRef = database.ref('sensor/tare');

    // Get data from Firebase
    weightRef.on('value', (snapshot) => {
      const weight = snapshot.val();
      document.getElementById('weight').innerText = `Weight: ${weight} kg`;

      const unitWeight = parseFloat(document.getElementById('unitWeight').value);
      if (unitWeight > 0) {
        const units = weight / unitWeight;
        document.getElementById('units').innerText = `Number of Units: ${units.toFixed(2)}`;
      }
    }, (error) => {
      console.error("Error fetching weight data: ", error);
    });

    // Function to tare the scale
    function tareScale() {
      tareRef.set(true).then(() => {
        alert('Scale tared successfully');
      }).catch((error) => {
        alert('Error taring scale: ' + error.message);
      });
    }
  </script>
</body>
</html>
