- 👋 Hi, I’m @somya795
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...
- 😄 Pronouns: ...
- ⚡ Fun fact: ...

<!---
somya795/somya795 is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
from flask import Flask, render_template_string, request import openai

app = Flask(name)

openai.api_key = 'your-api-key-here'  # Replace with your actual OpenAI API key

HTML_TEMPLATE = '''

<!DOCTYPE html><html>
<head>
    <title>NeuroGuru - AI Doubt Solver</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            padding: 30px;
            background-color: #f0f8ff;
            color: #333;
        }
        h1 {
            color: #2c3e50;
        }
        textarea {
            width: 100%;
            height: 100px;
            font-size: 16px;
        }
        select {
            margin-top: 10px;
            padding: 5px;
            font-size: 16px;
        }
        button {
            margin-top: 10px;
            padding: 10px 20px;
            font-size: 16px;
        }
        .answer {
            background-color: #ffffff;
            padding: 20px;
            border: 1px solid #ddd;
            margin-top: 20px;
        }
    </style>
</head>
<body>
    <h1>🧠 NeuroGuru</h1>
    <form method="POST">
        <textarea name="question" placeholder="Enter your question..." required></textarea><br>
        <select name="language">
            <option value="English">English</option>
            <option value="Hindi">Hindi</option>
        </select><br>
        <button type="submit">Solve</button>
    </form>{% if answer %}
    <div class="answer">
        <h2>Answer:</h2>
        <p>{{ answer }}</p>
    </div>
{% endif %}

</body>
</html>
'''@app.route('/', methods=['GET', 'POST']) def index(): answer = "" if request.method == 'POST': user_question = request.form['question'] language = request.form['language']

prompt = f"Answer this question in {language} with step-by-step explanation:\n{user_question}"

    response = openai.ChatCompletion.create(
        model="gpt-4",
        messages=[
            {"role": "user", "content": prompt}
        ]
    )

    answer = response['choices'][0]['message']['content']

return render_template_string(HTML_TEMPLATE, answer=answer)

if name == 'main': app.run(debug=True)

