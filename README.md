<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Barcode Scanner</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/quagga/0.12.1/quagga.min.js"></script>
    <style>
        /* General styling */
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            flex-direction: column;
            height: 100vh;
            background-color: #f4f6f9; /* Soft gray background */
        }

        .scanner-header {
            background-color: #2E8B57;
            color: white;
            padding: 10px 20px;
            text-align: center;
            font-size: 20px;
        }

        .scanner-container {
            display: flex;
            flex-direction: column;
            height: 100%;
        }

        /* Scanner view */
        #scanner {
            flex-grow: 2;
            background-color: black;
        }

        /* Scanned items list */
        .scanned-items {
            flex-grow: 1;
            background-color: white;
            border-top: 2px solid #ccc;
            padding: 10px;
            overflow-y: auto;
        }

        .scanned-items h3 {
            margin: 0 0 10px;
            color: #333;
        }

        .item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 10px;
            margin-bottom: 5px;
            background-color: #f9f9f9;
            border-radius: 5px;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .item-details {
            flex: 1;
        }

        .item-price, .item-quantity {
            margin-left: 10px;
            text-align: right;
            min-width: 70px;
        }

        .item-name {
            font-weight: bold;
        }
    </style>
</head>
<body>

    <!-- Header -->
    <div class="scanner-header">Scan Items</div>

    <!-- Scanner and Items Section -->
    <div class="scanner-container">
        <!-- Barcode Scanner -->
        <div id="scanner"></div>

        <!-- Scanned Items List -->
        <div class="scanned-items">
            <h3>Scanned Items</h3>
            <div id="item-list">
                <!-- Items will appear here -->
            </div>
        </div>
    </div>

    <script>
        // Simulated item database (you can replace this with your database or API calls)
        const itemDatabase = {
            "123456789012": { name: "Milk", price: 50 },
            "987654321098": { name: "Bread", price: 30 },
            "555555555555": { name: "Butter", price: 100 },
            // Add more items as needed
        };

        const itemListElement = document.getElementById("item-list");

        // Initialize QuaggaJS
        Quagga.init(
            {
                inputStream: {
                    name: "Live",
                    type: "LiveStream",
                    target: document.querySelector("#scanner"), // The scanner element
                },
                decoder: {
                    readers: ["ean_reader"], // EAN-13 barcodes
                },
            },
            function (err) {
                if (err) {
                    console.error(err);
                    alert("Error initializing scanner. Please try again.");
                    return;
                }
                Quagga.start();
            }
        );

        // Process detected barcodes
        Quagga.onDetected((data) => {
            const code = data.codeResult.code;
            if (itemDatabase[code]) {
                addItemToList(itemDatabase[code]);
            } else {
                alert("Item not found in the database!");
            }
        });

        // Add scanned item to the list
        function addItemToList(item) {
            const listItem = document.createElement("div");
            listItem.className = "item";

            const itemDetails = document.createElement("div");
            itemDetails.className = "item-details";
            itemDetails.innerHTML = `<div class="item-name">${item.name}</div>`;

            const itemPrice = document.createElement("div");
            itemPrice.className = "item-price">₹${item.price.toFixed(2)}</div>;

            const itemQuantity = document.createElement("div");
            itemQuantity.className = "item-quantity";
            itemQuantity.textContent = "1"; // Default quantity is 1

            listItem.appendChild(itemDetails);
            listItem.appendChild(itemPrice);
            listItem.appendChild(itemQuantity);
            itemListElement.appendChild(listItem);
        }
    </script>
</body>
</html>
