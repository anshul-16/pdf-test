// Example JSON object representing the date
const dateTimeJson = {
    "Chronology": "ISO",
    "DayOfMonth": 11,
    "DayOfWeek": 3, // Assuming 3 represents Wednesday (adjust as necessary)
    "DayOfYear": 254,
    "Hour": 13,
    "Minute": 45,
    "Second": 30,
    "MonthValue": 9,
    "Year": 2024
};

// Function to map day of week number to string
const getDayOfWeek = (dayOfWeek) => {
    const days = ["Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"];
    return days[dayOfWeek - 1]; // Adjusting index since array is 0-based
};

// Convert the JSON object to a readable date string
const readableDate = `${dateTimeJson.DayOfMonth.toString().padStart(2, '0')}-${dateTimeJson.MonthValue.toString().padStart(2, '0')}-${dateTimeJson.Year} ` +
    `${dateTimeJson.Hour.toString().padStart(2, '0')}:${dateTimeJson.Minute.toString().padStart(2, '0')}:${dateTimeJson.Second.toString().padStart(2, '0')} ` +
    `(${getDayOfWeek(dateTimeJson.DayOfWeek)})`;

console.log("Formatted DateTime: " + readableDate);
