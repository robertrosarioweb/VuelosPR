<style>
  footer {
    display: none !important;
  }
</style>
```js
import * as aq from "npm:arquero";
import * as Inputs from "https://cdn.jsdelivr.net/npm/@observablehq/inputs@0.12/+esm";
const Page = document.createElement("div");
Page.style.padding = "20px";
Page.style.width = "100%";
Page.style.height = "1400px";
// Set the width to be 80% of the browser width, or any value you prefer
Page.style.maxWidth = "1200px";  // Optional: Set a maximum width
Page.style.margin = "0";  // Ensure there's no margin on the Page itself
Page.style.backgroundColor = "gray";   // Set background color to gray
Page.style.position = "absolute";  // Position it absolutely
Page.style.top = "0px";  // Keep it at the top of the page
Page.style.left = "50%";  // Move it to the center horizontally
Page.style.transform = "translateX(-50%)";  // Adjust for the width of the Page to truly center it
document.body.appendChild(Page);  // Append Page to the body


// Make sure the body doesn't have any margin, so it starts at the very top of the viewport
document.body.style.margin = "0";
document.body.style.padding = "0";  // Remove any padding from body
document.body.style.minHeight = "100vh";  // Ensure body height is at least the f

// Create and style the title
const pageTitle = document.createElement("h1");
pageTitle.textContent = "Análisis de Vuelos entre Puerto Rico y Estados Unidos (2015 y 2017)";
pageTitle.style.textAlign = "center";
pageTitle.style.color = "black";
pageTitle.style.marginTop = "0";
pageTitle.style.paddingTop = "20px";
pageTitle.style.fontSize = "32px";
pageTitle.style.fontFamily = "Arial, sans-serif";
pageTitle.style.whiteSpace = "nowrap";
// Add the title to the top of the page container
const pageUrl = document.createElement("a");
pageUrl.textContent = "Fuente de datos: https://www.kaggle.com/datasets/bingecode/us-national-flight-data-2015-2020";
pageUrl.style.display = "block";

pageUrl.style.color = "black"; 
pageUrl.style.fontSize = "18px";
pageUrl.style.marginTop = "10px";
pageUrl.style.textDecoration = "none"; // Quitar su
pageUrl.style.whiteSpace = "nowrap";

Page.appendChild(pageTitle);
Page.appendChild(pageUrl);


// === Create container for inputs ===
const controlsContainer = document.createElement("div");
controlsContainer.style.display = "flex";
controlsContainer.style.justifyContent = "space-between";  // To distribute items evenly
controlsContainer.style.margin = "30px";
Page.appendChild(controlsContainer);  // Append controls inside Page

// === PR Map Inputs Container ===
const prInputsContainer = document.createElement("div");
prInputsContainer.style.flex = "1";  // Takes up 50% of the space
prInputsContainer.style.marginRight = "20px";  // Adds space between PR and US inputs

// === PR Map Inputs ===
const yearInputPR = Inputs.radio(["2015", "2017"], {
  value: "2015",
  label: "Select Year (Puerto Rico → USA)"
});
yearInputPR.style.marginBottom = "10px";
prInputsContainer.appendChild(yearInputPR);

const originInputPR = Inputs.radio(["San Juan", "Aguadilla", "Ponce"], {
  value: "San Juan",
  label: "Select Origin City (Puerto Rico)"
});
originInputPR.style.marginBottom = "20px";
prInputsContainer.appendChild(originInputPR);

// Append the PR inputs to the controls container
controlsContainer.appendChild(prInputsContainer);

// === US Map Inputs Container ===
const usInputsContainer = document.createElement("div");
usInputsContainer.style.flex = "0.5";  // Takes up 50% of the space

// === USA Map Inputs ===
const yearInputUS = Inputs.radio(["2015", "2017"], {
  value: "2015",
  label: "Select Year (USA → Puerto Rico)"
});
yearInputUS.style.marginBottom = "10px";
usInputsContainer.appendChild(yearInputUS);

const originInputUS = Inputs.radio(["San Juan", "Aguadilla", "Ponce"], {
  value: "San Juan",
  label: "Select Destination City (Puerto Rico)"
});
originInputUS.style.marginBottom = "20px";
usInputsContainer.appendChild(originInputUS);

// Append the US inputs to the controls container
controlsContainer.appendChild(usInputsContainer);

// === Flex container for maps ===
const mapWrapper = document.createElement("div");
mapWrapper.style.display = "flex";
mapWrapper.style.justifyContent = "space-between";
mapWrapper.style.marginTop = "10px";
Page.appendChild(mapWrapper);  // Append map wrapper inside Page

// === PR Map Container ===
const mapContainerPR = document.createElement("div");
mapContainerPR.id = "mapPR";
mapContainerPR.style.width = "48%";
mapContainerPR.style.height = "500px";
mapContainerPR.style.border = "1px solid #ccc";
mapWrapper.appendChild(mapContainerPR);

// === US Map Container ===
const mapContainerUS = document.createElement("div");
mapContainerUS.id = "mapUS";
mapContainerUS.style.width = "48%";
mapContainerUS.style.height = "500px";
mapContainerUS.style.border = "1px solid #ccc";
mapWrapper.appendChild(mapContainerUS);


// Create the chart container
const histogram1container = document.createElement('div');
histogram1container.style.border = '1px solid #ccc';
histogram1container.style.padding = '16px';
histogram1container.style.margin = '20px 0';
histogram1container.style.boxShadow = '0 2px 4px rgba(0,0,0,0.1)';
histogram1container.style.backgroundColor = 'white';
histogram1container.style.width = '545px';
histogram1container.style.height = '350px';
Page.appendChild(histogram1container);



// Create the container for the most cancelled city information
const cityInfoContainer = document.createElement('div');
cityInfoContainer.style.marginTop = '20px';
cityInfoContainer.style.padding = '16px';
cityInfoContainer.style.backgroundColor = 'white';
cityInfoContainer.style.boxShadow = '0 2px 4px rgba(0,0,0,0.1)';
cityInfoContainer.style.width = '545px';
cityInfoContainer.style.fontFamily = 'Arial, sans-serif';
Page.appendChild(cityInfoContainer);

// Year selector
const delayYearContainer = document.createElement("div");
delayYearContainer.style.flex = "1";
delayYearContainer.style.marginRight = "20px";

const yearInputdelay = Inputs.radio(["2015", "2017"], {
  value: "2015",
  label: "Select Year: "
});
yearInputdelay.style.marginBottom = "10px";
delayYearContainer.appendChild(yearInputdelay);
Page.appendChild(delayYearContainer);

// PR/USA toggle buttons
const buttonContainer = document.createElement('div');
buttonContainer.style.marginTop = '20px';
const btnPRtoUSA = document.createElement('button');
btnPRtoUSA.innerText = 'PR to USA';
btnPRtoUSA.style.marginRight = '10px';
btnPRtoUSA.style.padding = '8px 16px';
btnPRtoUSA.style.backgroundColor = 'light blue';
btnPRtoUSA.style.border = 'none';
btnPRtoUSA.style.borderRadius = '4px';
btnPRtoUSA.style.cursor = 'pointer';
btnPRtoUSA.style.marginTop = '-100px'; 
btnPRtoUSA.style.marginLeft = '130px'; // Corrected the casing here


const btnUSAToPR = document.createElement('button');
btnUSAToPR.innerText = 'USA to PR';
btnUSAToPR.style.padding = '8px 16px';
btnUSAToPR.style.backgroundColor = 'light red';
btnUSAToPR.style.border = 'none';
btnUSAToPR.style.borderRadius = '4px';
btnUSAToPR.style.cursor = 'pointer';
btnUSAToPR.style.marginTop = '-100px'; 
btnUSAToPR.style.marginLeft = '30px'; // No change needed here


buttonContainer.appendChild(btnPRtoUSA);
buttonContainer.appendChild(btnUSAToPR);
Page.appendChild(buttonContainer);


let filteredPR = [];
let filteredUSA = [];
let currentFilterType = 'PRtoUSA';

// Create the chart container
const histogram2container = document.createElement('div');
histogram2container.style.border = '1px solid #ccc';
histogram2container.style.padding = '16px';
histogram2container.style.margin = '20px 0';
histogram2container.style.boxShadow = '0 2px 4px rgba(0,0,0,0.1)';
histogram2container.style.backgroundColor = 'white';
histogram2container.style.width = '545px';
histogram2container.style.height = '350px';
// Positioning it to the right side of the page
histogram2container.style.marginTop = '-1450px';
histogram2container.style.alignSelf = 'flex-end'; // if inside a flex container
histogram2container.style.right = '-623px'; // Adjust this value to move the container further left or right
histogram2container.style.top = '809px'; // 
Page.appendChild(histogram2container);

// Year input
// Year input
const PromYearContainer = document.createElement("div");
PromYearContainer.style.flex = "1";

// Position it at the bottom-right of the page
PromYearContainer.style.position = "absolute";
PromYearContainer.style.right = "485px";
PromYearContainer.style.bottom = "60px";

const yearInputProm = Inputs.radio(["2015", "2017"], {
  value: "2015",
  label: "Select Year: "
});
yearInputProm.style.marginBottom = "10px";

PromYearContainer.appendChild(yearInputProm);
Page.appendChild(PromYearContainer);

// === Data fetch ===
let vuelosData = null;
async function fetchVuelos() {
  if (!vuelosData) {
    const csvData = await FileAttachment("./data/flights_2015-2017.csv").csv();
    vuelosData = aq.from(csvData);
  }
  return vuelosData;
}




// === City coordinates ===
const cityCoords = {
  "San Juan": { lat: 18.4655, lon: -66.1057 },
  "Aguadilla": { lat: 18.4949, lon: -67.1356 },
  "Ponce": { lat: 17.9783, lon: -66.9732 }
};

// === Category colors ===
const categoryColors = {
  "New York": "red", "Orlando": "blue", "Fort Lauderdale": "green",
  "Miami": "yellow", "Atlanta": "purple", "Newark": "orange",
  "Boston": "pink", "Tampa": "brown", "Philadelphia": "gray",
  "Chicago": "cyan", "Charlotte": "magenta", "Baltimore": "lime",
  "Houston": "indigo", "Charlotte Amalie": "teal", "Dallas/Fort Worth": "violet",
  "Washington": "turquoise", "Hartford": "maroon", "Christiansted": "olive",
  "Minneapolis": "silver", "Cleveland": "gold", "Detroit": "chocolate"
};

// === Utility: curved lines ===
function generateCurvePoints(origin, destination, numPoints = 10) {
  const points = [];
  for (let i = 0; i <= numPoints; i++) {
    const t = i / numPoints;
    const lat = origin.lat * (1 - t) + destination.lat * t;
    const lon = origin.lon * (1 - t) + destination.lon * t;
    const offset = Math.sin(Math.PI * t) * 5;
    points.push([lat + offset, lon]);
  }
  return points;
}

// === PR Map Logic ===
let mapPR = null;
function updateMapPR() {
  const year = yearInputPR.value;
  const originCity = originInputPR.value;

  fetchVuelos().then(table => {
    const filtered = table.filter(aq.escape(d => d.YEAR === year)).objects();
    renderMapPR(filtered, year, originCity);
  });
}

function renderMapPR(flights, year, originCity) {
  if (mapPR) mapPR.remove();
  mapPR = L.map("mapPR").setView([25.4655, -66.1057], 4);

  L.tileLayer(`https://api.mapbox.com/styles/v1/mapbox/satellite-streets-v12/tiles/512/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoibWVjb2JpIiwiYSI6IjU4YzVlOGQ2YjEzYjE3NTcxOTExZTI2OWY3Y2Y1ZGYxIn0.LUg7xQhGH2uf3zA57szCyw`, {
    attribution: '© Mapbox © OpenStreetMap'
  }).addTo(mapPR);

  const origin = cityCoords[originCity];
  L.circleMarker([origin.lat, origin.lon], {
    color: "green", fillColor: "green", fillOpacity: 0.8, radius: 10
  }).bindPopup(`${originCity}<br>Puerto Rico Origin`).addTo(mapPR);

  const counts = {}, coords = {};
  for (const flight of flights) {
    if (flight.ORIGIN_CITY_NAME === originCity) {
      const city = flight.DEST_CITY_NAME;
      counts[city] = (counts[city] || 0) + 1;
      if (!coords[city]) {
        coords[city] = { lat: parseFloat(flight.DEST_LAT), lon: parseFloat(flight.DEST_LON) };
      }
    }
  }

  Object.entries(counts).forEach(([city, count]) => {
    const destination = coords[city];
    const color = categoryColors[city] || "lightgray";
    const curve = generateCurvePoints(origin, destination);

    L.polyline(curve, { color, weight: 1, opacity: 0.7 }).addTo(mapPR);
    L.circleMarker([destination.lat, destination.lon], {
      color, fillColor: color, fillOpacity: 0.8, radius: Math.sqrt(count) * 0.3
    }).bindPopup(`${count} vuelos a ${city} desde ${originCity} (${year})`).addTo(mapPR);
  });
}

// === US Map Logic ===
let mapUS = null;
function updateMapUS() {
  const year = yearInputUS.value;
  const prCity = originInputUS.value;

  fetchVuelos().then(table => {
    const filtered = table.filter(aq.escape(d => d.YEAR === year)).objects();
    renderMapUS(filtered, year, prCity);
  });
}

function renderMapUS(flights, year, prCity) {
  if (mapUS) mapUS.remove();
  mapUS = L.map("mapUS").setView([25.4655, -66.1057], 4);

  L.tileLayer(`https://api.mapbox.com/styles/v1/mapbox/satellite-streets-v12/tiles/512/{z}/{x}/{y}@2x?access_token=pk.eyJ1IjoibWVjb2JpIiwiYSI6IjU4YzVlOGQ2YjEzYjE3NTcxOTExZTI2OWY3Y2Y1ZGYxIn0.LUg7xQhGH2uf3zA57szCyw`, {
    attribution: '© Mapbox © OpenStreetMap'
  }).addTo(mapUS);

  const destination = cityCoords[prCity];
  L.circleMarker([destination.lat, destination.lon], {
    color: "green", fillColor: "green", fillOpacity: 0.8, radius: 10
  }).bindPopup(`${prCity}<br>Puerto Rico Destination`).addTo(mapUS);

  const counts = {}, coords = {};
  for (const flight of flights) {
    if (flight.DEST_CITY_NAME === prCity) {
      const city = flight.ORIGIN_CITY_NAME;
      counts[city] = (counts[city] || 0) + 1;
      if (!coords[city]) {
        coords[city] = { lat: parseFloat(flight.ORIGIN_LAT), lon: parseFloat(flight.ORIGIN_LON) };
      }
    }
  }

  Object.entries(counts).forEach(([city, count]) => {
    const origin = coords[city];
    const color = categoryColors[city] || "lightgray";
    const curve = generateCurvePoints(origin, destination);

    L.polyline(curve, { color, weight: 1, opacity: 0.7 }).addTo(mapUS);
    L.circleMarker([origin.lat, origin.lon], {
      color, fillColor: color, fillOpacity: 0.8, radius: Math.sqrt(count) * 0.3
    }).bindPopup(`${count} vuelos desde ${city} a ${prCity} (${year})`).addTo(mapUS);
  });
}

// === Initial Renders ===
const initialFlights = (await fetchVuelos()).filter(aq.escape(d => d.YEAR === "2015")).objects();
renderMapPR(initialFlights, "2015", "San Juan");
renderMapUS(initialFlights, "2015", "San Juan");

// === Event Listeners ===
yearInputPR.addEventListener("change", updateMapPR);
originInputPR.addEventListener("change", updateMapPR);
yearInputUS.addEventListener("change", updateMapUS);
originInputUS.addEventListener("change", updateMapUS);



// Update city info container with the most cancelled city
function updateCityInfoContainer(mostCancelledCity, count, filterType) {
 let displayText = `<span style="font-size: 15px;">City with Most Cancelled Flights: <br/> ${mostCancelledCity} - ${count} cancellations</span>`;

if (filterType === 'PRtoUSA') {
  displayText = `<span style="font-size: 15px;">Most Cancellations from PR to USA: <br/> ${mostCancelledCity} - ${count} cancellations</span>`;
} else if (filterType === 'USAToPR') {
  displayText = `<span style="font-size: 15px;">Most Cancellations from USA to PR: <br/> ${mostCancelledCity} - ${count} cancellations</span>`;
}

  cityInfoContainer.innerHTML = displayText;
  cityInfoContainer.style.border = '1px solid #ccc'; // Light gray border
  cityInfoContainer.style.padding = '10px';
  
  cityInfoContainer.style.backgroundColor = '#fff';
  cityInfoContainer.style.boxShadow = '0 4px 8px rgba(0, 0, 0, 0.1)';
}

// Draw graph with selected data and year
function DelayGraph(filteredData = filteredPR, filterType = 'PRtoUSA', year = '2015') {
  const cancelledFlights = filteredData.filter(d => d.CANCELLED === "1");

  let cancelledFlightsByCity;
  let cancelledFlightsByCarrier;
  let plotTitle = '';

  // Filter cancelled flights based on the selected type
  if (filterType === 'PRtoUSA') {
    cancelledFlightsByCity = d3.rollups(
      cancelledFlights,
      v => v.length,
      d => d.DEST_CITY_NAME
    );
    plotTitle = `Cancelled Flights in ${year}: Puerto Rico to USA`;
  } else if (filterType === 'USAToPR') {
    cancelledFlightsByCity = d3.rollups(
      cancelledFlights,
      v => v.length,
      d => d.ORIGIN_CITY_NAME
    );
    plotTitle = `Cancelled Flights in ${year}: USA to Puerto Rico `;
  }

  // Sort cities by number of cancellations
  const sortedCancelledFlights = cancelledFlightsByCity.sort((a, b) => b[1] - a[1]);
  const data = sortedCancelledFlights.map(([city, count]) => ({ city, count }));

  // Get the city with the most cancellations
  const mostCancelledCity = sortedCancelledFlights[0] ? sortedCancelledFlights[0][0] : 'N/A';
  const mostCancelledCount = sortedCancelledFlights[0] ? sortedCancelledFlights[0][1] : 0;

  // Filter cancelled flights for the most cancelled city (based on filterType)
  let cancelledFlightsInMostCancelledCity;
  if (filterType === 'PRtoUSA') {
    cancelledFlightsInMostCancelledCity = cancelledFlights.filter(d => d.DEST_CITY_NAME === mostCancelledCity);
  } else if (filterType === 'USAToPR') {
    cancelledFlightsInMostCancelledCity = cancelledFlights.filter(d => d.ORIGIN_CITY_NAME === mostCancelledCity);
  }

  // Group cancelled flights in the most cancelled city by carrier
  const cancelledFlightsByCarrierInCity = d3.rollups(
    cancelledFlightsInMostCancelledCity,
    v => v.length,
    d => d.OP_UNIQUE_CARRIER
  );

  // Sort by number of cancellations to find the carrier with the most cancellations
  const sortedCancelledFlightsByCarrierInCity = cancelledFlightsByCarrierInCity.sort((a, b) => b[1] - a[1]);
  const mostCancelledCarrier = sortedCancelledFlightsByCarrierInCity[0] ? sortedCancelledFlightsByCarrierInCity[0][0] : 'N/A';
  const mostCancelledCarrierCount = sortedCancelledFlightsByCarrierInCity[0] ? sortedCancelledFlightsByCarrierInCity[0][1] : 0;

  // Get the carrier name (full name)
  const carrierNames = {
    "AA": "American Airlines",
    "B6": "JetBlue Airways",
    "DL": "Delta Air Lines",
    "UA": "United Airlines",
    "WN": "Southwest Airlines",
    "AS": "Alaska Airlines",
    "F9": "Frontier Airlines",
    "NK": "Spirit Airlines",
    "HA": "Hawaiian Airlines",
    "VX": "Virgin America",
    "EV": "ExpressJet Airlines",
    "MQ": "Envoy Air",
    "OH": "PSA Airlines",
    "YV": "Mesa Airlines",
    // Add more as needed
  };

  const mostCancelledCarrierName = carrierNames[mostCancelledCarrier] || mostCancelledCarrier;

  // Update the city info container with the most cancelled city and carrier
  updateCityInfoContainer(mostCancelledCity, mostCancelledCount, filterType);

  // Create and display the carrier info for the most cancelled city
  const carrierInfoBox = document.createElement("div");
  carrierInfoBox.style.marginTop = "10px";
  carrierInfoBox.style.padding = "8px";
  carrierInfoBox.style.border = "1px solid #ddd";
  carrierInfoBox.style.backgroundColor = "#f9f9f9";
  carrierInfoBox.style.fontSize = "14px";

  carrierInfoBox.innerHTML = `
    In ${mostCancelledCity}, the carrier with the Most Cancellations: ${mostCancelledCarrierName} (${mostCancelledCarrierCount} cancellations)
  `;

  // Add the carrier info to the same container that holds the city info
  cityInfoContainer.appendChild(carrierInfoBox);

  // Create the chart plot for cancelled flights by city
  const redColorScale = d3.scaleSequential(d3.interpolateReds).domain([0, d3.max(data, d => d.count)]);
  const CountPlot = Plot.plot({
    marks: [
      Plot.barX(data, {
        x: "count",
        y: "city",
        fill: d => redColorScale(d.count),
        sort: { y: "x", reverse: true }
      })
    ],
    title: plotTitle,
    x: { label: "Count of Cancelled Flights" },
    y: { label: "City" },
    width: 700,
    height: 400,
    marginLeft: 100,
    marginBottom: 80
  });

  histogram1container.innerHTML = ''; // Clear previous plot
  histogram1container.appendChild(CountPlot);
}



// Filter data based on selected year, then draw chart
async function updateFilteredDataAndDraw() {
  const table = await fetchVuelos();
  const year = yearInputdelay.value;

  filteredPR = table
    .filter(aq.escape(d => d.YEAR === year && d.ORIGIN_STATE_ABR === "PR"))
    .objects();

  filteredUSA = table
    .filter(aq.escape(d => d.YEAR === year && d.ORIGIN_STATE_ABR !== "PR"))
    .objects();

  const dataToUse = currentFilterType === 'PRtoUSA' ? filteredPR : filteredUSA;
  DelayGraph(dataToUse, currentFilterType, year);
}

// Button click handlers
btnPRtoUSA.addEventListener("click", () => {
  currentFilterType = 'PRtoUSA';
  btnPRtoUSA.classList.add("active");
  btnUSAToPR.classList.remove("active");
  DelayGraph(filteredPR, currentFilterType, yearInputdelay.value);
});

btnUSAToPR.addEventListener("click", () => {
  currentFilterType = 'USAToPR';
  btnUSAToPR.classList.add("active");
  btnPRtoUSA.classList.remove("active");
  DelayGraph(filteredUSA, currentFilterType, yearInputdelay.value);
});

// Year change handler
yearInputdelay.addEventListener("change", () => {
  updateFilteredDataAndDraw();
});

// Initialize
btnPRtoUSA.classList.add("active"); // Default active button
updateFilteredDataAndDraw();        // Initial graph

// Main graph logic
function PromGraph() {
  fetchVuelos().then(table => {
    const filteredPromPR = table.filter(aq.escape(d => d.YEAR === yearInputProm.value &&
      d.ORIGIN_STATE_ABR === "PR"
    )).objects();
    const filteredPromUSA = table.filter(aq.escape(d => d.YEAR === yearInputProm.value &&
      d.ORIGIN_STATE_ABR !== "PR"
    )).objects();

    const vuelosNoCanceladosPR = filteredPromPR.filter(flight => 
      flight.CANCELLED !== "1" && flight.DEP_DELAY_NEW !== "0.0"
    );
    const vuelosNoCanceladosUSA = filteredPromUSA.filter(flight => 
      flight.CANCELLED !== "1" && flight.DEP_DELAY_NEW !== "0.0"
    );
    const totalRetrasoUSA = vuelosNoCanceladosUSA.reduce((acc, flight) => {
  return acc + parseFloat(flight.DEP_DELAY_NEW);
}, 0);
const totalRetrasoPR = vuelosNoCanceladosPR.reduce((acc, flight) => {
  return acc + parseFloat(flight.DEP_DELAY_NEW);
}, 0);
 const totalVuelosPR = vuelosNoCanceladosPR.length;
 const totalVuelosUSA = vuelosNoCanceladosUSA.length;
 const retrasoPromedioGlobalPR = totalRetrasoPR / totalVuelosPR;
  const retrasoPromedioGlobalUSA = totalRetrasoUSA / totalVuelosUSA;

const retrasosPorMesPR = filteredPromPR.reduce((acc, flight) => {
  if (flight.CANCELLED !== "1" && flight.DEP_DELAY_NEW !== "0.0") {
    const añoMesPR = `${flight.YEAR}-${flight.MONTH}`; // Group by year and month
    const retrasoPR = parseFloat(flight.DEP_DELAY_NEW);

    if (!acc[añoMesPR]) {
      acc[añoMesPR] = { totalRetrasoPR: 0, cantidadVuelosPR: 0 };
    }

    acc[añoMesPR].totalRetrasoPR += retrasoPR;
    acc[añoMesPR].cantidadVuelosPR += 1;
  }
  return acc;
}, {});

const retrasosPorMesUSA = filteredPromUSA.reduce((acc, flight) => {
  if (flight.CANCELLED !== "1" && flight.DEP_DELAY_NEW !== "0.0") {
    const añoMesUSA = `${flight.YEAR}-${flight.MONTH}`; // Group by year and month
    const retrasoUSA = parseFloat(flight.DEP_DELAY_NEW);

    if (!acc[añoMesUSA]) {
      acc[añoMesUSA] = { totalRetrasoUSA: 0, cantidadVuelosUSA: 0 };
    }

    acc[añoMesUSA].totalRetrasoUSA += retrasoUSA;
    acc[añoMesUSA].cantidadVuelosUSA += 1;
  }
  return acc;
}, {});

const retrasosPromedioPorMesPR = Object.keys(retrasosPorMesPR).map(añoMesPR => {
  const [yearPR, monthPR] = añoMesPR.split('-').map(Number);
  const paddedMonthPR = monthPR.toString().padStart(2, '0');
  const formattedYearMonthPR = `${yearPR}-${paddedMonthPR}`;
  
  const { totalRetrasoPR, cantidadVuelosPR } = retrasosPorMesPR[añoMesPR];
  const avgDelay = totalRetrasoPR / cantidadVuelosPR;

  return {
    yearMonth: formattedYearMonthPR,
    avgDelay
  };
});

const retrasosPromedioPorMesUSA = Object.keys(retrasosPorMesUSA).map(añoMesUSA => {
  const [yearUSA, monthUSA] = añoMesUSA.split('-').map(Number);
  const paddedMonthUSA = monthUSA.toString().padStart(2, '0');
  const formattedYearMonthUSA = `${yearUSA}-${paddedMonthUSA}`;
  
  const { totalRetrasoUSA, cantidadVuelosUSA } = retrasosPorMesUSA[añoMesUSA];
  const avgDelay = totalRetrasoUSA / cantidadVuelosUSA;

  return {
    yearMonth: formattedYearMonthUSA,
    avgDelay
  };
});

  const promgraph = Plot.plot({
    title: "Average Monthly Delay: PR vs USA",
  marks: [
    // Line and dots for Puerto Rico (PR)
    Plot.line(retrasosPromedioPorMesPR, {
      x: "yearMonth",
      y: "avgDelay",
      stroke: "#68c3c0", // teal
      strokeWidth: 2
    }),
    Plot.dot(retrasosPromedioPorMesPR, {
      x: "yearMonth",
      y: "avgDelay",
      fill: "#68c3c0",
      size: 5
    }),

    // Line and dots for USA
    Plot.line(retrasosPromedioPorMesUSA, {
      x: "yearMonth",
      y: "avgDelay",
      stroke: "#ff6b6b", // coral/red
      strokeWidth: 2
    }),
    Plot.dot(retrasosPromedioPorMesUSA, {
      x: "yearMonth",
      y: "avgDelay",
      fill: "#ff6b6b",
      size: 5
    })
  ],
  x: {
    label: "Mes (Año)",
    tickRotate: 45,
    tickValues: retrasosPromedioPorMesUSA.map(d => d.yearMonth),
    tickFormat: d => d
  },
  y: {
    label: "Retraso Promedio (minutos)"
  },
  width: 800,
  height: 400,
  marginBottom: 80
});

histogram2container.innerHTML = ""; // Clear old plot if needed
histogram2container.appendChild(promgraph);
// Create custom legend
const legend = document.createElement("div");
legend.style.position = "absolute";
legend.style.top = "10px";
legend.style.right = "5px";
legend.style.backgroundColor = "white";
legend.style.border = "1px solid #ccc";
legend.style.borderRadius = "4px";
legend.style.padding = "6px 10px";
legend.style.fontSize = "14px";
legend.style.boxShadow = "0 2px 4px rgba(0, 0, 0, 0.1)";
legend.innerHTML = `
  <div style="margin-bottom: 4px;"><span style="display:inline-block;width:12px;height:12px;background-color:#68c3c0;margin-right:6px;border-radius:2px;"></span>Puerto Rico (PR)</div>
  <div><span style="display:inline-block;width:12px;height:12px;background-color:#ff6b6b;margin-right:6px;border-radius:2px;"></span>USA</div>
`;

// Make the container relative so legend is positioned correctly
histogram2container.style.position = "relative";

// Append legend
histogram2container.appendChild(legend);

const carrierNames = {
  "AA": "American Airlines",
  "B6": "JetBlue Airways",
  "DL": "Delta Air Lines",
  "UA": "United Airlines",
  "WN": "Southwest Airlines",
  "AS": "Alaska Airlines",
  "F9": "Frontier Airlines",
  "NK": "Spirit Airlines",
  "HA": "Hawaiian Airlines",
  "VX": "Virgin America",
  "EV": "ExpressJet Airlines",
  "MQ": "Envoy Air",
  "OH": "PSA Airlines",
  "YV": "Mesa Airlines",
  // Add more as needed
};

// Step 1: Calculate total delay and count of flights for each carrier
const allFlights = [...vuelosNoCanceladosPR, ...vuelosNoCanceladosUSA];
const delaysByCarrier = {};

for (const flight of allFlights) {
  const carrier = flight.OP_UNIQUE_CARRIER;
  const delay = parseFloat(flight.DEP_DELAY_NEW);

  if (!delaysByCarrier[carrier]) {
    delaysByCarrier[carrier] = { totalDelay: 0, flightCount: 0 };
  }

  delaysByCarrier[carrier].totalDelay += delay;
  delaysByCarrier[carrier].flightCount += 1;
}

// Step 2: Compute average and find carrier with highest mean delay
let maxCarrier = null;
let maxMeanDelay = 0;

for (const carrier in delaysByCarrier) {
  const { totalDelay, flightCount } = delaysByCarrier[carrier];
  const meanDelay = totalDelay / flightCount;

  if (meanDelay > maxMeanDelay) {
    maxCarrier = carrier;
    maxMeanDelay = meanDelay;
  }
}

// Step 3: Show result in text box
const delayInfoBox = document.createElement("div");
delayInfoBox.style.marginTop = "69px";
delayInfoBox.style.padding = "10px";
delayInfoBox.style.border = "1px solid #999";
delayInfoBox.style.width = "554px";
delayInfoBox.style.height= "50px";
delayInfoBox.style.backgroundColor = "#f9f9f9";
delayInfoBox.style.fontSize = "15px";
delayInfoBox.style.marginLeft = "-19px";

const carrierFullName = carrierNames[maxCarrier] || maxCarrier;
delayInfoBox.textContent = ` Carrier with the highest average departure delay: ${carrierFullName} (${maxMeanDelay.toFixed(2)} min)`;





// Append it below the chart container
histogram2container.appendChild(delayInfoBox);

    console.log("Total PR", retrasosPromedioPorMesPR );
    console.log("Total USA", retrasosPromedioPorMesUSA );
    //console.log(" Total USA", retrasoPromedioGlobalUSA);
    // Optionally, update chart inside `histogram2container` here
  });
}

// 🔁 Add event listener to update graph when year changes
yearInputProm.addEventListener("input", PromGraph);

// Initial render
PromGraph();



