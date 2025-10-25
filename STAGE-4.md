New GitHub issue that meets all the objectives. This will create the issue in the repository.

---

### **Issue Title**

`Generate 'Hello World' Flask Application Files`

### **Issue Body**

We need to create the application code for our new Python web service. This will be a simple "Hello World" application using the Flask framework.

The project should include the following files:

* `app/main.py`: This file should contain the main Flask application.
    * Import `Flask`.
    * Create a Flask app instance.
    * Define a single route `/` (the root) that returns the JSON object `{"message": "Hello World"}`.
    * In the `if __name__ == "__main__":` block, run the app on host `0.0.0.0` and port `5000` with debugging enabled.

* `requirements.txt`: This file should list the Python dependencies.
    * It must include `Flask` (e.g., `Flask==2.3.2`).
    * It should also include `gunicorn` (e.g., `gunicorn==20.1.0`) for production.

Please generate the content for these two files (`app/main.py` and `requirements.txt`) based on the descriptions above.

---

### **Issue Label**

`autocoder-bot`

<img width="1884" height="957" alt="image" src="https://github.com/user-attachments/assets/54e2e765-9b15-4381-a2eb-1138a35cf50f" />
