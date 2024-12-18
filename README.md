## Using Chaquopy to Run Python in Your Android App

### Step 1: Add Chaquopy to Your Project

1. **Open your Android Studio project.**

2. **Modify your project’s `build.gradle` files:**

   #### In the Project-Level `build.gradle`
   Ensure the `google()` repository is added:

   ```gradle
   buildscript {
       repositories {
           google()
           mavenCentral()
       }
       dependencies {
           classpath "com.android.tools.build:gradle:YOUR_VERSION"
           classpath "com.chaquopy.gradle:com.chaquopy.gradle.plugin:14.0.1" // Add this line
       }
   }
   ```

   #### In the App-Level `build.gradle`
   Apply the Chaquopy plugin and add its dependency:

   ```gradle
   plugins {
       id 'com.android.application'
       id 'com.chaquopy.python' // Add Chaquopy plugin
   }

   android {
       compileSdk 33

       defaultConfig {
           applicationId "com.example.myapp"
           minSdk 21
           targetSdk 33
           versionCode 1
           versionName "1.0"

           // Add this line to specify the Python version
           python {
               buildPython "3.9" // Replace with the version you want (3.8, 3.9, etc.)
           }
       }

       ...
   }

   dependencies {
       implementation "com.chaquopy:chaquopy:14.0.1"
   }
   ```

---

### Step 2: Add Python Scripts

1. **Create a directory for Python scripts:**
   - Go to `src/main` in your project.
   - Create a new folder named `python`.

   The directory structure should look like this:
   ```
   src/main/python
   ```

2. **Add your Python script to the folder:**
   
   Example: Create a file `src/main/python/automate.py`:
   ```python
   def process_data(data):
       return f"Processed: {data}"
   ```

---

### Step 3: Access Python Code from Java/Kotlin

Chaquopy provides an API to call Python code from your app.

#### Example: Calling Python Script in Kotlin

1. **Use Chaquopy's `Python` API to load and call your Python script:**

   ```kotlin
   import com.chaquopy.python.PyObject
   import com.chaquopy.python.Python

   fun runPythonScript(inputData: String): String {
       // Get an instance of the Python environment
       val py = Python.getInstance()

       // Load the Python module (filename without ".py")
       val pyModule = py.getModule("automate")

       // Call the Python function
       val result: PyObject = pyModule.callAttr("process_data", inputData)

       // Convert the result to a string
       return result.toString()
   }
   ```

2. **Use this function anywhere in your app:**

   ```kotlin
   val processedData = runPythonScript("Sample Input")
   Log.d("PythonResult", processedData) // Output: "Processed: Sample Input"
   ```

---

### Step 4: Run the App

1. **Build the app in Android Studio.** Chaquopy will automatically handle dependencies for Python.
2. **Test the app** by triggering the function that calls your Python script.

---

### Step 5: Adding Python Libraries

If your Python script requires third-party libraries (e.g., `numpy`, `pandas`):

1. **Specify the libraries in the `build.gradle`:**
   ```gradle
   python {
       pip {
           install "numpy"
           install "pandas"
       }
   }
   ```

2. **Rebuild the project,** and Chaquopy will automatically download and package the dependencies.

---

### Step 6: Debugging

1. Check the **Logcat** for any errors related to Python execution.
2. Ensure Python scripts are placed in the `src/main/python` directory.

---

### Example Use Case: Automating File Processing

1. **Create a Python script (`src/main/python/file_processor.py`):**
   ```python
   import os

   def list_files(directory):
       return os.listdir(directory)
   ```

2. **Call it from Kotlin:**
   ```kotlin
   val py = Python.getInstance()
   val pyModule = py.getModule("file_processor")
   val fileList: PyObject = pyModule.callAttr("list_files", "/path/to/directory")
   Log.d("PythonFiles", fileList.toString())
   ```

---

### Next Steps

- Expand your Python scripts for the automation tasks you need.
- Use Chaquopy to combine Python’s powerful libraries with Android features.

Let me know if you need further assistance!

