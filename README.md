## requirements
- XAMPP
- VS code

## Step 1:
Go to xampp controll panel and start both of apache and MySQL

![Screenshot 2024-07-24 181157](https://github.com/user-attachments/assets/6168a9c8-5b56-4fb7-a264-98b9c00a4532)

## note:
if you keep geting an error when you trying to start mySQL.. here is an amazing solution that i found
- open the Task manager in your PC.
- then go to (Services)
- look for (mySQL80) and you will probably find it running, just stop it, and everything will work fine (At least in my case).
  
## step 2: 
- go to C>xampp>htdocs
- create a folder inside (htdocs) and give it a name.
- inside the folder that you have just created, create your project files (example: php file)
- open that file in VS code.

## step 3: create a database and table and some columns

<img width="959" alt="image" src="https://github.com/user-attachments/assets/5e2f4b28-0bb2-484b-97a7-6404da0fa1fc">


## step 4:
- start writing code and testing your work.
- here is my basic code:
```
  <?php
// Database connection details
$servername = "localhost";
$username = "root";
$password = "";
$dbname = "robot_control";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

// Handle POST request
if ($_SERVER["REQUEST_METHOD"] == "POST" && isset($_POST['command'])) {
    $command = $_POST['command'];

    // Prepare SQL statement to insert command into database
    $sql = "INSERT INTO commands (command, created_at) VALUES ('$command', NOW())";

    if ($conn->query($sql) === TRUE) {
        echo "Command '$command' sent successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}

$conn->close();
?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Robot Control Panel</title>
    <style>
        .button-container {
            display: flex;
            justify-content: space-around;
            margin-top: 20px;
        }
        button {
            padding: 10px 20px;
            font-size: 16px;
            cursor: pointer;
        }
    </style>
</head>
<body>
    <h1>Robot Control Panel</h1>
    
    <div class="button-container">
        <button onclick="sendCommand('forward')">Forward</button>
        <button onclick="sendCommand('backward')">Backward</button>
        <button onclick="sendCommand('left')">Left</button>
        <button onclick="sendCommand('right')">Right</button>
        <button onclick="sendCommand('stop')">Stop</button>
    </div>

    <script>
        function sendCommand(direction) {
            // Send command to backend PHP script
            var xhttp = new XMLHttpRequest();
            xhttp.onreadystatechange = function() {
                if (this.readyState == 4 && this.status == 200) {
                    console.log(this.responseText);  // Log response from server
                }
            };
            xhttp.open("POST", "control.php", true);
            xhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
            xhttp.send("command=" + direction);
        }
    </script>
</body>
</html>
```

## finally 

![Screenshot 2024-07-24 175854](https://github.com/user-attachments/assets/b728590c-b337-432c-83b8-afa3de07edfd)


  
