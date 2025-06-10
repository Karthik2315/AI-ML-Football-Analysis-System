# AI-ML Football Analysis System

## ⚽ Intelligent Football Video Analysis Powered by AI/ML

This project introduces an AI-driven system designed to analyze football match videos and extract valuable analytical insights. Leveraging state-of-the-art computer vision and machine learning techniques, this system provides detailed statistics and observations about player performance, team dynamics, and camera movements within a match.

### 🚀 Key Features

* **Player Speed Tracking:** Accurately calculates and reports the speed of individual players throughout the video.
* **Team Ball Control Analysis:** Quantifies ball possession and control for each team, offering insights into their dominance.
* **Camera Movement Analysis:** Identifies and analyzes camera movements (e.g., panning, zooming) to understand the video's framing and focus.
* **Automated Workflow:** Simply place your video in the designated input folder, and the system processes it, storing the analysis in the output folder.
* **Modular Design:** Built with a `main.py` for core logic and a dedicated `models` directory for AI models, ensuring clean organization.

### 🎥 How It Works (High-Level)

The system processes football videos by:

1.  **Custom YOLO Model Training:** A crucial first step involves training a custom YOLO (You Only Look Once) object detection model specifically on a football-focused dataset (e.g., from Roboflow) to accurately detect players and the ball.
2.  **Object Detection:** The trained YOLO model identifies players, the ball, and potentially other relevant objects in each video frame.
3.  **Tracking:** Robust tracking algorithms are applied to follow detected objects across frames, enabling continuous monitoring of player positions and ball trajectory.
4.  **Feature Extraction:** Based on the tracking data, the system extracts key metrics:
    * **Player Speed:** Calculated from changes in player position over time.
    * **Ball Control:** Determined by tracking ball possession periods for each team.
    * **Camera Movements:** Analyzed by observing the scene's motion relative to the objects.
5.  **Output Generation:** The extracted analytics are then presented, typically by annotating the input video with bounding boxes, speed readings, and on-screen statistics, which are saved to the output folder.

### ⚙️ Installation & Model Training

To set up and run this project locally, you'll need to install dependencies and **train your custom YOLO model**.

1.  **Clone the repository:**
    ```bash
    git clone [https://github.com/your-username/AI-ML-Football-Analysis-System.git](https://github.com/your-username/AI-ML-Football-Analysis-System.git)
    cd AI-ML-Football-Analysis-System
    ```
    (Replace `your-username` with your actual GitHub username or the repository owner's username.)

2.  **Create a Virtual Environment (Recommended):**
    It's highly recommended to use a virtual environment to manage dependencies and avoid conflicts with other Python projects.
    ```bash
    python -m venv venv
    # On Windows:
    .\venv\Scripts\activate
    # On macOS/Linux:
    source venv/bin/activate
    ```

3.  **Install Dependencies:**
    All required libraries are listed in `requirements.txt`.
    ```bash
    pip install -r requirements.txt
    ```

4.  **YOLO Model Training with Roboflow:**
    This system requires a YOLO model *trained specifically on a football dataset*. We recommend using [Roboflow](https://roboflow.com/) for dataset management and easy model training.

    * **a. Roboflow Account & Dataset:**
        * Go to [Roboflow.com](https://roboflow.com/) and create a free account or log in.
        * Search for a suitable "football" or "soccer" detection dataset. Roboflow hosts many public datasets. If you have your own annotated data, you can upload it.
        * Once you have a dataset, select it and choose your desired YOLO format for export (e.g., `YOLOv8 PyTorch`, `YOLOv5 PyTorch`). You'll need your dataset's **API Key** for download.

    * **b. Download Dataset Locally:**
        Install the Roboflow Python package:
        ```bash
        pip install roboflow
        ```
        Then, use the provided Python snippet from your Roboflow dataset page to download it. It will look something like this:
        ```python
        # Create a temporary Python script (e.g., download_dataset.py)
        # and paste the Roboflow download code into it.
        # Example (your actual code will be from Roboflow's UI):
        from roboflow import Roboflow
        rf = Roboflow(api_key="YOUR_ROBOFLOW_API_KEY") # Replace with your actual API key
        project = rf.workspace("your-workspace").project("your-football-project")
        dataset = project.version(1).download("yolov8") # Or "yolov5", etc.
        ```
        Run this script (`python download_dataset.py`). This will download the dataset to a directory like `your-football-project-1` in your project root.

    * **c. Install YOLO Framework & Train:**
        The `main.py` script is designed to be flexible with YOLO versions, but popular choices include `ultralytics` (for YOLOv8) or `YOLOv5` by Glenn Jocher.

        **For YOLOv8 (Recommended, using `ultralytics`):**
        ```bash
        pip install ultralytics
        ```
        Now, train your model using the downloaded dataset. The `data.yaml` file (e.g., `your-football-project-1/data.yaml`) downloaded from Roboflow contains paths to your images and labels.
        ```bash
        # Example training command (adjust epochs, model, batch size as needed)
        # Ensure you are in the project root where `main.py` is located.
        # The 'data' argument points to the data.yaml from your downloaded dataset.
        yolo train model=yolov8n.pt data=your-football-project-1/data.yaml epochs=50 imgsz=640 project=runs/train name=football_detection
        ```
        This command will save trained weights (like `best.pt`) in `runs/train/football_detection/weights/`.

    * **d. Place the Trained Model:**
        After successful training, you will find your best-performing model weights (usually `best.pt` or similar) in the `runs/train/YOUR_TRAINING_NAME/weights/` directory.

        * **Create the `models` directory:**
            ```bash
            mkdir models
            ```
        * **Move your trained model:** Copy or move the `best.pt` file into the `models/` directory.
            ```bash
            cp runs/train/football_detection/weights/best.pt models/
            ```
        Now, your project structure should have `models/best.pt`.

### 🚀 Usage

1.  **Prepare your input video:**
    Place the football match video file (e.g., `match_footage.mp4`) you wish to analyze into the `Input_video/` directory. Create this folder if it doesn't exist.
    ```bash
    mkdir Input_video
    # Then copy your video file into it
    cp /path/to/your/video.mp4 Input_video/
    ```

2.  **Run the analysis:**
    Ensure your virtual environment is active and your trained model is in `models/`.
    Execute the main script from your project root:
    ```bash
    python main.py
    ```
    The system will automatically detect the video in `Input_video/`, process it using your trained model, and save the output.

3.  **View the output:**
    After the script completes, the analyzed video and any generated data files will be stored in the `Output_video/` directory. Create this folder if it doesn't exist.
    ```bash
    mkdir Output_video
    ```
    The output will typically include an annotated video with player bounding boxes, speed readings, and potentially on-screen statistics.

### 📂 Project Structure
