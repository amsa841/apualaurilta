<!DOCTYPE html>
<html lang="fi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Laurilta apua</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 0;
            display: flex;
            justify-content: center;
            align-items: center;
            height: 100vh;
            background-color: #f4f4f9;
        }

        .queue-container {
            width: 80%;
            max-width: 600px;
            background-color: white;
            padding: 20px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
            text-align: center;
        }

        h2 {
            margin-bottom: 20px;
        }

        .queue-list {
            list-style-type: none;
            padding: 0;
            margin: 0;
        }

        .queue-item {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin: 5px 0;
            padding: 10px;
            border: 1px solid #ccc;
            border-radius: 4px;
            box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        }

        .queue-item span {
            font-weight: bold;
        }

        .green {
            color: green;
        }

        .red {
            color: red;
        }

        .green-btn {
            background-color: green;
            color: white;
            padding: 5px 10px;
            cursor: pointer;
            border-radius: 4px;
            margin: 5px;
        }

        .green-btn:hover {
            background-color: darkgreen;
        }

        .red-btn {
            background-color: red;
            color: white;
            padding: 5px 10px;
            cursor: pointer;
            border-radius: 4px;
            margin: 5px;
        }

        .red-btn:hover {
            background-color: darkred;
        }

        input {
            padding: 10px;
            margin-bottom: 10px;
            width: 80%;
            max-width: 400px;
            border-radius: 4px;
            border: 1px solid #ccc;
        }

        button {
            padding: 10px 20px;
            background-color: #4CAF50;
            color: white;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            margin-top: 10px;
        }

        button:hover {
            background-color: #45a049;
        }

        .refresh-btn {
            background-color: #f44336;
            color: white;
            margin-top: 20px;
        }

        .refresh-btn:hover {
            background-color: #d32f2f;
        }

    </style>
</head>
<body>

    <div class="queue-container">
        <h2>Laurilta apua</h2>
        <input id="nameInput" type="text" placeholder="Syötä nimesi">
        <button onclick="addName()">Lisää nimi</button>
        <button class="refresh-btn" onclick="refreshQueue()">Päivitä lista</button>
        <ul id="queueList" class="queue-list"></ul>
    </div>

    <script>
        // Function to get the queue data from localStorage or initialize as an empty array
        function getQueueData() {
            const queue = localStorage.getItem('queueData');
            return queue ? JSON.parse(queue) : [];
        }

        // Function to save the queue data to localStorage
        function saveQueueData(queueData) {
            localStorage.setItem('queueData', JSON.stringify(queueData));
        }

        // Function to render the queue list
        function renderQueue() {
            const queueData = getQueueData();
            const queueList = document.getElementById('queueList');
            queueList.innerHTML = '';  // Clear the existing list

            // Loop through the queue data and create a list item for each person
            queueData.forEach((student, index) => {
                const li = document.createElement('li');
                li.classList.add('queue-item');

                const nameSpan = document.createElement('span');
                nameSpan.textContent = student.name;

                // Set the color of the name based on whether the student got help
                if (student.gotHelp) {
                    nameSpan.classList.add('green');
                } else {
                    nameSpan.classList.add('red');
                }

                // Create the buttons for updating the status
                const greenBtn = document.createElement('button');
                greenBtn.textContent = 'Sain apua';
                greenBtn.classList.add('green-btn');

                const redBtn = document.createElement('button');
                redBtn.textContent = 'En tarvitse enää apua';
                redBtn.classList.add('red-btn');

                // Event listeners for the buttons to update the status
                greenBtn.addEventListener('click', () => {
                    const queueData = getQueueData();
                    queueData.splice(index, 1);  // Remove the student from the list after help
                    saveQueueData(queueData);  // Save the updated queue data to localStorage
                    renderQueue();  // Re-render the queue after updating the status
                });

                redBtn.addEventListener('click', () => {
                    const queueData = getQueueData();
                    queueData.splice(index, 1);  // Remove the student from the list
                    saveQueueData(queueData);  // Save the updated queue data to localStorage
                    renderQueue();  // Re-render the queue after updating the status
                });

                // Append elements to the list item
                li.appendChild(nameSpan);
                li.appendChild(greenBtn);
                li.appendChild(redBtn);

                // Append list item to the list
                queueList.appendChild(li);
            });
        }

        // Function to add a name to the queue
        function addName() {
            const nameInput = document.getElementById('nameInput');
            const name = nameInput.value.trim();

            if (name) {
                const queueData = getQueueData();
                const student = {
                    name: name,
                    gotHelp: false
                };
                queueData.push(student);
                saveQueueData(queueData);  // Save the updated queue data to localStorage
                nameInput.value = '';  // Clear the input field
                renderQueue();  // Re-render the queue after adding the name
            } else {
                alert('Ole hyvä ja syötä nimesi.');
            }
        }

        // Function to refresh (clear) the queue
        function refreshQueue() {
            localStorage.removeItem('queueData');  // Remove all queue data from localStorage
            renderQueue();  // Re-render the queue (it will be empty)
        }

        // Event listener for when localStorage changes in another tab
        window.addEventListener('storage', function(event) {
            if (event.key === 'queueData') {
                renderQueue();  // Re-render the queue if localStorage changes
            }
        });

        // Add event listener to handle the Enter key for adding the name
        document.getElementById('nameInput').addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                addName();  // Add the name when Enter is pressed
            }
        });

        // Initial render of the queue
        renderQueue();
    </script>

</body>
</html>
