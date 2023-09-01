CHAT GPT PROMPT & FEEDBACK

----------

Default (GPT-3.5)

User
create a javascript code that asks 5 questions and saves them to a database
ChatGPT
Creating a complete JavaScript application that asks 5 questions and saves them to a database is a bit more complex than can be fully explained in a single response. It involves setting up a front-end interface for collecting user input, creating a back-end server to handle database operations, and integrating the two. Below, I'll provide you with a simplified example that you can use as a starting point, but keep in mind that a real-world application would require more robust code and security measures.

In this example, we'll use Node.js and MongoDB as the database. Before running the code, you'll need to install Node.js and set up a MongoDB database.

Create a project folder and initialize it with npm (Node Package Manager):
bash
Copy code
mkdir question-app
cd question-app
npm init -y
Install the necessary dependencies (Express and Mongoose):
bash
Copy code
npm install express mongoose
Create the app.js file for the server:
javascript
Copy code
const express = require('express');
const mongoose = require('mongoose');
const bodyParser = require('body-parser');

const app = express();
const port = process.env.PORT || 3000;

mongoose.connect('mongodb://localhost/questiondb', {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});

const Question = mongoose.model('Question', {
  text: String,
  answer: String,
});

app.use(bodyParser.urlencoded({ extended: true }));
app.use(bodyParser.json());

app.post('/api/questions', (req, res) => {
  const { text, answer } = req.body;

  if (!text || !answer) {
    return res.status(400).json({ message: 'Both text and answer are required.' });
  }

  const question = new Question({ text, answer });

  question.save((err) => {
    if (err) {
      return res.status(500).json({ message: 'Internal Server Error' });
    }

    res.status(201).json({ message: 'Question saved successfully' });
  });
});

app.listen(port, () => {
  console.log(`Server is running on port ${port}`);
});
Run the server:
bash
Copy code
node app.js
Now, create a front-end HTML file (e.g., index.html) for asking questions:
html
Copy code
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Question App</title>
</head>
<body>
    <h1>Question App</h1>
    <form id="questionForm">
        <label for="text">Question Text:</label>
        <input type="text" id="text" name="text" required>
        <br>
        <label for="answer">Answer:</label>
        <input type="text" id="answer" name="answer" required>
        <br>
        <button type="submit">Submit</button>
    </form>
    <script>
        document.getElementById('questionForm').addEventListener('submit', async (e) => {
            e.preventDefault();

            const text = document.getElementById('text').value;
            const answer = document.getElementById('answer').value;

            const response = await fetch('/api/questions', {
                method: 'POST',
                headers: {
                    'Content-Type': 'application/json',
                },
                body: JSON.stringify({ text, answer }),
            });

            const data = await response.json();
            alert(data.message);

            document.getElementById('text').value = '';
            document.getElementById('answer').value = '';
        });
    </script>
</body>
</html>
Open the index.html file in your web browser. You should see a simple form to input questions and answers. When you submit a question, it will be sent to the server and saved in the database.
Remember that this is just a basic example to get you started. In a real-world application, you would need to handle validation, user authentication, error handling, and potentially more complex database interactions. Additionally, for security reasons, you should never expose your database connection details in the front-end code as shown here; this should be handled securely on the server.
