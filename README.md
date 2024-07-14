---

# Object Tracking and Segmentation Tool

This repository contains a Jupyter Notebook that uses the ultralytics library to perform object tracking and segmentation on a video using the YOLOv8 model.

## Requirements

Before running the notebook, you need to install the necessary packages:

```bash
pip install ultralytics
```

## Overview of the Script

This notebook contains a script that uses the ultralytics library to perform object tracking and segmentation on a video. Here’s a quick overview of what the code does:

### Imports Libraries:
- **logging**: Manages messages and errors.
- **collections**: Provides the defaultdict for storing tracking history.
- **cv2**: Handles video processing.
- **ultralytics**: Loads and uses the YOLOv8 model for tracking and segmentation.
- **ultralytics.utils.plotting**: Provides tools for annotating frames with bounding boxes and masks.

### Defines Functions:
- **`load_model(model_path)`**: Loads the YOLOv8 model from the specified path.
- **`process_frame(model, frame)`**: Processes each video frame to detect, track, and annotate objects with bounding boxes and segmentation masks.
- **`main(video_path, model_path, output_path)`**: Manages the overall workflow, including loading the video and model, processing each frame, and saving the annotated video.

### Main Function:
- **`main()`**:
  - Collects paths for the input video, model, and output video.
  - Loads the YOLOv8 model.
  - Opens the video file and initializes the video writer for the output.
  - Processes each frame of the video to detect and annotate objects.
  - Writes the annotated frames to the output video file.

## How to Use the Notebook

### Run the Notebook:
- Execute the cells to run the script.
- In Google Colab or Jupyter Notebook, click 'Run' or press `Shift + Enter`.

### Provide Information:
- **Paths**: Specify the paths for the input video, YOLOv8 model, and the output video file.
  - Example paths:
    - `video_path = "/content/input_video_.mp4"`
    - `model_path = "yolov8n-seg.pt"`
    - `output_path = "/content/output_video.avi"`

### See the Result:
- The annotated video will be saved to the specified output path.
- You can view the video with the detected and tracked objects, each annotated with bounding boxes and segmentation masks.

## Example Usage

Here is a brief example of how you can use the object tracking and segmentation tool:

1. **Import Necessary Libraries**:
    ```python
    import logging
    from collections import defaultdict
    import cv2
    from ultralytics import YOLO
    from ultralytics.utils.plotting import Annotator, colors
    from google.colab.patches import cv2_imshow
    ```

2. **Initialize Logging**:
    ```python
    logging.basicConfig(level=logging.INFO, format='%(asctime)s - %(levelname)s - %(message)s')
    ```

3. **Load the YOLO Model**:
    ```python
    def load_model(model_path):
        model = YOLO(model_path)
        logging.info("Model loaded successfully.")
        return model
    ```

4. **Process a Frame**:
    ```python
    def process_frame(model, frame):
        annotator = Annotator(frame, line_width=2)
        results = model.track(frame, persist=True)
        for result in results:
            track_id = result.id
            bbox = result.box
            mask = result.mask
            annotator.box_label(bbox, f"ID: {track_id}", color=colors(track_id, True))
            annotator.seg_bbox(mask=mask, mask_color=colors(track_id, True), track_label=str(track_id))
        return annotator.result()
    ```

## Contributing

Contributions are welcome! If you have any suggestions or improvements, feel free to open an issue or submit a pull request.

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---
