<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Simulated Climate Data</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <style>
        body {
            font-family: 'Arial', sans-serif;
            background-color: #f4f7fa;
            color: #453;
            margin: 0;
            padding: 0;
        }
        .container {
            max-width: 900px;
            margin: 50px auto;
            background-color: #ffffff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }
        h1 {
            text-align: center;
            color: #4caf50;
            margin-bottom: 20px;
        }
        .form-group {
            margin-bottom: 20px;
        }
        label {
            font-size: 18px;
            font-weight: 600;
            color: #333;
            margin-bottom: 5px;
            display: block;
        }
        input[type="number"] {
            width: 100%;
            padding: 12px;
            font-size: 16px;
            border: 2px solid #ccc;
            border-radius: 8px;
            margin-top: 5px;
        }
        button {
            background-color: #4caf50;
            color: white;
            font-size: 16px;
            padding: 12px 20px;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            width: 100%;
            margin-top: 15px;
            transition: background-color 1.0s;
        }
        button:hover {
            background-color: #45a049;
        }
        .suggestions {
            background-color: #f9f9f9;
            border-radius: 8px;
            padding: 15px;
            margin-top: 30px;
        }
        .suggestions h3 {
            font-size: 20px;
            color: #333;
        }
        .suggestions p {
            font-size: 16px;
            color: #555;
            margin: 5px 0;
        }
        .chart-container {
            margin-top: 30px;
        }
        #climateChart {
            max-width: 100%;
            height: 400px;
        }
        footer {
            text-align: center;
            margin-top: 50px;
            padding: 20px;
            background-color: #333;
            color: white;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>Simulated Climate Data and Suggestions</h1>
    
    <!-- User input form -->
    <div class="form-group">
        <label for="startYear">Enter Starting Year:</label>
        <input type="number" id="startYear" placeholder="Starting Year" min="1900" max="2100">
    </div>
    <div class="form-group">
        <label for="endYear">Enter Ending Year:</label>
        <input type="number" id="endYear" placeholder="Ending Year" min="1900" max="2100">
    </div>
    <button onclick="generateData()">Generate Data</button>

    <!-- Display climate suggestions -->
    <div id="suggestions" class="suggestions"></div>

    <!-- Plot the climate data -->
    <div class="chart-container">
        <canvas id="climateChart"></canvas>
    </div>
</div>

<!-- Footer section -->
<footer>
    <p>&copy; 2024 Simulated Climate Data | All rights reserved</p>
</footer>

<script>
// Function to generate simulated climate data based on the year
function generateClimateData(year) {
    // Seed random number generator with year for reproducibility
    const random = Math.sin(year * 0.1) * 10000;
    const temperature = (random % 25 + 10);  // Temperature between 10°C and 35°C
    const rainfall = (random % 1300 + 200);  // Rainfall between 200mm and 1500mm
    const humidity = (random % 60 + 30);  // Humidity between 30% and 90%

    return { temperature, rainfall, humidity };
}

// Function to provide basic suggestions based on simulated climate data
function provideSuggestions(temperature, rainfall, humidity) {
    let tempSuggestion = '';
    if (temperature > 30) {
        tempSuggestion = "It's hot! Stay hydrated.";
    } else if (temperature < 10) {
        tempSuggestion = "It's cold! Wear warm clothes.";
    } else {
        tempSuggestion = "It's moderate weather.";
    }

    let rainfallSuggestion = '';
    if (rainfall > 1200) {
        rainfallSuggestion = "It's a very rainy year!";
    } else if (rainfall < 500) {
        rainfallSuggestion = "It's a dry year!";
    } else {
        rainfallSuggestion = "Rainfall is moderate this year.";
    }

    let humiditySuggestion = '';
    if (humidity > 70) {
        humiditySuggestion = "It's quite humid this year.";
    } else if (humidity < 40) {
        humiditySuggestion = "The air is dry this year.";
    } else {
        humiditySuggestion = "Humidity levels are moderate.";
    }

    return { tempSuggestion, rainfallSuggestion, humiditySuggestion };
}

// Function to generate data and display suggestions and chart
function generateData() {
    const startYear = parseInt(document.getElementById("startYear").value);
    const endYear = parseInt(document.getElementById("endYear").value);
    if (isNaN(startYear) || isNaN(endYear) || startYear > endYear) {
        alert("Please enter valid years.");
        return;
    }

    const years = [];
    const temperatures = [];
    const rainfalls = [];
    const humidities = [];
    const suggestions = [];

    // Generate simulated climate data for each year in the range
    for (let year = startYear; year <= endYear; year++) {
        const { temperature, rainfall, humidity } = generateClimateData(year);
        const { tempSuggestion, rainfallSuggestion, humiditySuggestion } = provideSuggestions(temperature, rainfall, humidity);

        years.push(year);
        temperatures.push(temperature);
        rainfalls.push(rainfall);
        humidities.push(humidity);
        suggestions.push(`
            <h3>Suggestions for ${year}:</h3>
            <p>Temperature: ${tempSuggestion}</p>
            <p>Rainfall: ${rainfallSuggestion}</p>
            <p>Humidity: ${humiditySuggestion}</p>
        `);
    }

    // Display suggestions
    document.getElementById("suggestions").innerHTML = suggestions.join('');

    // Create the chart using Chart.js
    const ctx = document.getElementById("climateChart").getContext("2d");
    new Chart(ctx, {
        type: 'line',
        data: {
            labels: years,
            datasets: [
                {
                    label: 'Temperature (°C)',
                    data: temperatures,
                    borderColor: 'red',
                    fill: false,
                    tension: 0.1
                },
                {
                    label: 'Annual Rainfall (mm)',
                    data: rainfalls,
                    borderColor: 'blue',
                    fill: false,
                    tension: 0.1
                },
                {
                    label: 'Humidity (%)',
                    data: humidities,
                    borderColor: 'green',
                    fill: false,
                    tension: 0.1
                }
            ]
        },
        options: {
            responsive: true,
            plugins: {
                legend: {
                    position: 'top',
                },
                tooltip: {
                    mode: 'index',
                    intersect: false,
                }
            },
            scales: {
                y: {
                    beginAtZero: false
                }
            }
        }
    });
}
</script>

</body>
</html>
