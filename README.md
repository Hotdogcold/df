# df

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Shopping Cart</title>
    <script>
        // JavaScript to focus on the barcode input when the page loads
        window.onload = function() {
            document.getElementById('barcode').focus();
        };
    </script>
</head>
<body>
    <h1>Barcode Scanner</h1>

    <!-- Form to input barcode -->
    <form action="barcode.php" method="POST" id="form">
        <label for="barcode">Scan your barcode:</label>
        <input type="text" name="barcode" id="barcode" required>
        <button type="submit">Submit</button>
    </form>

    <div>
        <h2>Shopping Cart</h2>
        <?php
        $items = [
            "0000000036" => ["name" => "dave", "price" => 1],
            "0000000041" => ["name" => "harold", "price" => 0.5],
            
        ];

        
        $total_bill = 0;

        
        if ($_SERVER['REQUEST_METHOD'] == 'POST' && isset($_POST['barcode'])) {
            $barcode = trim($_POST['barcode']);

            
            if ($barcode == "endcode") {
                echo "<p>Shopping done</p>";
            } else {
               
                if (array_key_exists($barcode, $items)) {
                    $item = $items[$barcode];
                    echo "<p>This is: " . $item['name'] . ", price is: " . $item['price'] . "</p>";

                    
                    $total_bill += $item['price'];
                } else {
                    echo "<p>Item not found: $barcode</p>";
                }
            }
        }

        echo "<p>Your total bill is: $total_bill</p>";
        ?>
    </div>
</body>
</html>
