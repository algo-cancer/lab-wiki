# Setting up Nextflow Language Server on VS Code Runnning on Biowulf

This will give you debugging and syntax error highlighting in VS Code for nextflow. If you are running nextflow locally, you shouldnt have any problem, just install the Nextflow extension. However, if you are running VS code on an interactive job on Biowulf and accessing through ssh, you need to change some configurations to use a newer version of Java:

### 1. Modify the VS Code Terminal Profile
- Open VS Code.
- Go to **File > Preferences > Settings** (or press `Ctrl+,`).
- Search for `terminal.integrated.profiles` and click **Edit in settings.json**.
- Add or update the terminal profile configuration as follows:
  ```json
  "terminal.integrated.profiles.linux": {
    "bash": {
      "path": "/bin/bash",
      "args": ["--login", "-c", "module load java/22.0.2 && bash"]
    }
  },
  "terminal.integrated.defaultProfile.linux": "bash"
  ```

### 2. Set Environment Variables for the Language Server
- Open **File > Preferences > Settings** (or press `Ctrl+,`).
- Search for `terminal.integrated.env.linux` and click **Edit in settings.json**.
- Add the following environment variables:
  ```json
  "terminal.integrated.env.linux": {
    "JAVA_HOME": "/usr/local/java/jdk-22.0.2",
    "PATH": "/usr/local/java/jdk-22.0.2/bin:${env:PATH}"
  }
  ```

### 3. Configure the Java Home for the Nextflow Extension
- Open **File > Preferences > Settings** (or press `Ctrl+,`).
- Search for `Nextflow › Java: Home`.
- Click **Edit in settings.json** to add the following:
  ```json
  "nextflow.java.home": "/usr/local/java/jdk-22.0.2"
  ```

### 4. Restart the Nextflow Language Server
- Open the **Command Palette** (press `Ctrl+Shift+P` or `Cmd+Shift+P` on macOS).
- Type and select `Nextflow: Restart Language Server`.
- If that option isn’t available, select `Developer: Reload Window`.

### 5. Verify the Setup
- Open a terminal in VS Code (`Ctrl+`` or View > Terminal`).
- Run:
  ```bash
  java -version
  ```
- Confirm the output shows:
  ```plaintext
  java version "22.0.2"
  ```
