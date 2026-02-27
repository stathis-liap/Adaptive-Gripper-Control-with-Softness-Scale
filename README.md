Adaptive Gripper Control System

This project provides a complete pipeline for a vision-assisted robotic gripper using a BLDC motor, YOLO object detection, and red marker tracking to determine object grip feasibility. It includes real-time visualization, PID autotuning, and dimension transmission to a microcontroller.
📁 Project Structure
🔧 Core Scripts
main.py

Entry point for launching the gripper vision pipeline. It initializes and runs the Pipeline class with default parameters including YOLO model path and target object class.
pipeline.py

The main logic that:

-Initializes YOLO and camera input

-Detects red markers in the scene to estimate gripper width

-Detects objects and estimates their dimensions from RGB-D data

-Determines if an object is grippable based on bounding box overlap and width comparison

-Sends the dimensions to the microcontroller

**📷 Vision and Input Modules**

**_inputFromCamera.py_**

Handles RGB or RGB-D input from either webcam or dataset images. Supports fallback mock depth data and dataset replay.

**_detectGripper.py_**

A test utility that:

-Tracks red markers in the image

-Computes their centroid positions

-Estimates distance between them for debugging or visualization

**📡 Communication**

**_dimension_sender.py_**

-Handles serial communication to send object dimensions (length_cm, width_cm) to the microcontroller (e.g., an XMC board). Used in the pipeline to transmit graspable object data.

**🧠 Configuration and Models**

**_configYOLO.py_**

-Contains class names used by YOLO.

-Loads YOLOv8 model for object detection using the Ultralytics API.

-Includes font and color constants for overlay drawing.

**_configDepth.py_**

-Contains functions to normalize and extract average depth from a depth map.

-Essential for computing real-world object dimensions from pixel sizes.

**_config.h_ (for microcontroller firmware)**

-C-style config header to toggle features:

-Magnetic sensor

-Commander interface

Angle reading

🧪 Autotuning

_**pid_autotune_magnetic.py**_

-Python script for autotuning PID values for motor torque control using magnetic field readings from a sensor (e.g., TLx493D).

-Iteratively sends PID values

-Evaluates response based on magnetic data trends

-Saves the best-performing PID combination

⚙️ Firmware

_**controller_v2.ino**_

Arduino-compatible firmware that interfaces with the motor, sensors, and PID logic. Responds to serial commands sent by the GUI or pid_autotune_magnetic.py.

🚀 How to Run

_Requirements_

**Python ≥ 3.8**

Packages:

    pip install opencv-python numpy pyserial ultralytics

YOLOv8 weights (place your .pt file at yolo-weights/yolov8n.pt)

Optionally, a dataset at dataset/ for offline mode

_Run the Full Pipeline_

    python main.py

_Run PID Autotuning_

    python pid_autotune_magnetic.py

_Test Red Marker Detection_

    python detectGripper.py

**💬 Notes**

-Press q in any OpenCV window to exit.

-Serial port (COM3, COM5, etc.) must be updated based on your hardware setup.

-The system supports real-time analysis and closed-loop feedback between vision and actuation.

