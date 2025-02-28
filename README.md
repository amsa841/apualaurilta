# Laurilta Apua - Queue Management System

This is a simple web-based queue management system designed to help students request and track assistance from a teacher or assistant named Lauri. The system allows students to add their names to a queue, mark themselves as having received help, or remove themselves from the queue if they no longer need assistance.

## Features

- **Add Your Name**: Students can enter their names into the queue.
- **Mark as Helped**: Students can mark themselves as having received help, which removes them from the queue.
- **Remove from Queue**: Students can remove themselves from the queue if they no longer need assistance.
- **Persistent Storage**: The queue data is stored in the browser's `localStorage`, so it persists even if the page is refreshed.
- **Real-Time Updates**: If the queue is updated in another browser tab, the changes are reflected in real-time.

## How to Use

1. **Add Your Name**: Enter your name in the input field and click the "Lisää nimi" button or press Enter.
2. **Mark as Helped**: Once you receive help, click the "Sain apua" button next to your name to remove yourself from the queue.
3. **Remove from Queue**: If you no longer need help, click the "En tarvitse enää apua" button to remove yourself from the queue.
4. **Refresh the Queue**: Use the "Päivitä lista" button to clear the entire queue.

## Code Structure

- **HTML**: The structure of the page, including the input field, buttons, and queue list.
- **CSS**: Styling for the page, including colors, buttons, and layout.
- **JavaScript**: Logic for managing the queue, including adding names, updating status, and saving data to `localStorage`.

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/laurilta-apua.git
