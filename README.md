# GUI for Keithley 2614B SourceMeter Interaction

Welcome to the GUI tool designed specifically for the Keithley 2614B SourceMeter instrument! 

This tool serves as an interface for students and professors at the Fakultät für Elektrotechnik und Informationstechnik on the Technische Universität Ilmenau. 

With its user-friendly interface, it simplifies tasks and operations related to the SourceMeter, making the experience seamless and more intuitive for both beginners and seasoned users!

### References:
- [ ] [Series 2600B System SourceMeter User Manual](https://gitlab.tu-ilmenau.de/-/ide/project/FakEI/it-tet/supraleitende-elektronik/measurement_gui/tree/main/-/Series%202600B%20Manuals/2600BS-900-01A_2600B_Users_Aug2021.pdf/)
- [ ] [Series 2600B System SourceMeter Reference Manual](https://gitlab.tu-ilmenau.de/-/ide/project/FakEI/it-tet/supraleitende-elektronik/measurement_gui/tree/main/-/Series%202600B%20Manuals/2600BS-901-01F_2600B_Reference_Aug2021.pdf/)

This Jupyter Notebook project was created in a **_Conda_** environment in **_Visual Studio Code_** on the **3.11** version of _Python_.

## Installation & Setup Guide

If you would like to further debug and examine the code in depth, I recommend the following setup. Otherwise, navigate to the **Startup** step. 

### 1. **Setting up the Environment**:

#### Using Conda:

- **Installing Conda**:
  - If you do not have Conda installed:
    - Download and install **Miniconda** (a minimal Conda installer) or **Anaconda** (a larger distribution that includes Conda, Python, and many scientific packages).
      - [Miniconda Installation Guide](https://docs.conda.io/en/latest/miniconda.html)
      - [Anaconda Installation Guide](https://docs.anaconda.com/anaconda/install/)
  
- **Creating a Conda Environment**:
  - Once Conda is installed, open a terminal or command prompt.
  - Create a new environment:
    ```
    conda create --name your_env_name python=3.8
    ```
    > Replace `your_env_name` with a suitable name for your environment. 
  - Activate the environment:
    ```
    conda activate your_env_name
    ```

### 2. **Installing the Necessary Packages**:

With your Conda environment activated, install the required packages:

```bash
conda install ipywidgets
conda install -c anaconda json
pip install qcodes matplotlib pandas numpy 
```

### 3. **Setting up Visual Studio Code (VSCode)**:

- **Installing VSCode**:
  - If you don't have VSCode:
    - Download and install from [VSCode Official Site](https://code.visualstudio.com/)

- **Setting up Jupyter in VSCode**:
  - Launch VSCode.
  - Go to Extensions (or press `Ctrl+Shift+X`).
  - Search for and install the `Python` extension provided by Microsoft.
  - Open your notebook in VSCode.
  - Select your Conda environment as the Python interpreter. Do this by clicking on the Python version shown on the bottom-left of the status bar and selecting your environment from the list.

### 4. **Additional Setup**:

To connect and use the Keithley 2614B instrument, it is important to install [**NI-VISA**](https://www.ni.com/de/support/downloads/drivers/download.ni-visa.html#484351) on the computer which will be the server of the Notebook. 

### 5. **Startup**: 

To startup the GUI, navigate to the folder which contains the cloned repository, open up Command Prompt by tiping `cmd` in the _File Explorer_ and pressing `Enter` on your keyboard. 

In the Command Prompt, write the following : 
```
conda activate 
voila GUI_app.ipynb
```

The server should now be started up in your default browser!

> If you don't have Voila installed on your PC, after activating your base conda environment simply write `pip install voila`. You should now be able to initiate the GUI. 


## Imports and dependencies

Before we dive into the functionality and operation of the GUI, it's essential to understand the libraries and modules that have been imported. These modules provide the core functionalities that power the GUI and interact with the Keithley 2614B SourceMeter.

### Libraries Overview:

- _QCoDeS Instrument Drivers_: The **Keithley2614B driver** is utilized to communicate and control the Keithley 2614B SourceMeter.

- _QCoDeS Core and Widgets_: Libraries such as _qcodes, qcodes.station, and qcodes.interactive_widget_ offer foundational functionalities and interactive widgets for the GUI.

- _QCoDeS Dataset_: Facilitates measurement data handling, plotting, and storing within the QCoDeS ecosystem.

- _IPython_: Enhances the interface with display tools, like clear output and HTML display features. The %matplotlib widget magic command allows for interactive plots in the notebook.

- _ipywidgets_: Offers interactive GUI widgets for Python.

- General Python Libraries: _time, threading, sys, io, logging, json, and os_ libraries provide utilities ranging from time operations, threading for concurrency, system operations, file handling, and logging functionalities.

- _matplotlib_: Used for plotting functionalities in the notebook.

- _NumPy_: Provides mathematical tools and array operations crucial for data processing.

## **Device & User Interaction Widgets:**

- **Username Input**:
   - A `Label` and `Text` widget combination to get the username.

- **Device Control**:
   - Buttons to `Connect`, `Disconnect`, `Exit`, and `Clear` (output). 
   - A `Dropdown` widget to toggle the displayed channels (`SMUA`, `SMUB`, or both).

- **Data Management**:
   - `Refresh Button` to refresh the data overview.
   - `Export Button` to export datasets to a CSV file.

### Functionalities:

- **Connecting to the device**:
    - To connect to the device, input your _username_, and press the `Connect` button. Communication with the device is handled by the `connect-device` function. You should see a message in the `output` box that confirms you have successfully connected to the device.
    - In case of a `VisaIOError`, please make sure the device is **powered on** before attempting to connect!

- **Disconnecting from the device**:
    - The `disconnect_device` function removes the connected device from the station. The current user will also be logged out of the session. Before shutting the server down and closing the web app, **make sure to disconnect from the device!**
    > In case the device isn't disconnected before ending session, please restart the Python kernel

#### Widget Callbacks:
    - `on_connect`: Handles device connection, sets up channels, updates the UI, and initializes/creates a database for data storage.
    - `on_disconnect`: Resets the UI to its original state and disconnects from the device.
    - `on_exit_key`: Appears to send an exit command to the device.
    - `on_clear`: Clears the output widget.
    - `adjust_channels`: Adjusts the active measurement channels based on the dropdown value.
    - `on_refresh`: Refreshes the data overview.
    - `on_export`: Exports datasets to a CSV file.

## **Channel widgets and measurement functions**

### Components:

1. **Source Configuration:**
   - `Source Range Dropdown` : A dropdown widget to select the source range. Please refer to the [Series 2600B User's Manual](https://gitlab.tu-ilmenau.de/-/ide/project/FakEI/it-tet/supraleitende-elektronik/measurement_gui/tree/main/-/Series%202600B%20Manuals/2600BS-900-01A_2600B_Users_Aug2021.pdf/) for further information.
   - `Source Mode Dropdown` : A dropdown widget to select the source mode. You can choose between "Voltage" and "Current". 

2. **Sweep Configuration:**
   - `Sweep Range Slider` : A slider to select the range of values for the sweep.
   - `Step Selector`: A widget to input the step for the sweep range.

3. **Control Buttons:**
   - `Run Button` : Initiates the sweep.
   - `Stop Button` : Halts the sweep operation if active and turns the SMU output off. 

4. **Limit Configuration:**
   - `Limit Function Dropdown` : Choose between `Power` and `Primary` as a limit type (indicates voltage or current based on the selected source mode).
   - `Compliance Setting` : A widget to input the compliance value. 
   > REFER TO THE INSTRUMENT MANUAL FOR SAFETY REGARDING COMPLIANCE LIMIT, SOURCING AND MEASURING. 

5. **Progress Indication:**
   - `Progress Bar`: A bar that shows the progress of the ongoing sweep operation.

6. **Direct Source Control:**

   - `Direct Source Input` : Allows direct input for setting the source voltage/current level.
   - `ON Button` : Turns the source ON.
   - `OFF Button` : Turns the source OFF.
   - `Set Button` : Applies the value from the Direct Source Input.

7. **Additional Settings:**

   - `Wire Sense Buttons` : Choose between '2-Wire' (local sensing) and '4-Wire' (remote sensing). 
   - `Fast Sweep Button` : Activate fast sweep mode.
   - `Mode Select Radio Buttons` : Choose between _IV_, _VI_, and _VIfourprobe_. When selecting _IV_ make sure the `Source Mode` is set to _Voltage_. For _VI and VIfourprobe_, `Source Mode` must be set to _Current_.


### Functionalities: 

- **Sourcing Mode Selection** (`on_sourcemode_select`): Determines whether the source measure unit is in voltage or current mode.
  
- **Source Range Selection** (`on_sourcerange_select`): Sets the appropriate range for sourcing, based on the user's dropdown choice.
  
- **Limit Function Selection** (`on_limitfunc_select`): Determines how the SMU limits its operation, either based on power or primary (current/voltage).
  
- **Limit Compliance Adjustment** (`on_limit_compliance`): Changes the compliance limit of the SMU based on the user input.
  
- **Fast Sweep Operations** (`on_fast_sweep`): Performs a quick measurement sweep based on user-defined parameters. 

- **Stopping the Operation** (`on_stop`): Provides the user the ability to cancel a measurement sweep in progress.
  
- **Warning Dialog** (`show_dialog`): Displays a warning dialog if the compliance set is too low, potentially preventing device damage.
  
- **Setting Sensing Mode** (`on_set_sensemode`): Allows the user to switch between 4-Wire (remote sensing) and 2-Wire (local sensing) modes.
  
- **Running Measurements** (`run_measurements`): This is the main operation function. It sets up and starts the sweep measurements. Measurements are stored in a database. It also provides a progress bar and handles errors and cancellations.
  
- **Directly Setting Source** (`on_set_clicked`): Allows direct setting of a specific voltage or current value on the SMU.
  
- **Turning SMU ON/OFF** (`on_on_clicked` & `on_off_clicked`): Basic functions to turn the SMU output on or off.

## Potential known errors and troubleshooting

### Station already has a component named 'keith'

- Occurs when attempting to connect to the device upon next startup. In order to fix this, simply restart the Python kernel. To further prevent this, before shutting down the notebook, please disconnect from the device!

### PyVISA timeout error

- Occurs when attempting to connect to the device. Known cause of this is attempting to connect when the device isn't powered on. Make sure to power on the SourceMeter before connecting the GUI to the device.

### Attribute error : Station has no component ('keith')

- Usually occurs along with the PyVISA timeout error, but occurs whenever the `setup_channel()` function gets called. In case of this error persisting further debugging and troubleshooting is needed. 

### Device error : QUEUE FULL -350

- Could possibly occur when a larger sweep is ran while also trying to change some other setting on the device. This error shouldn't occur, in case it does, try clicking the `EXIT` button and attempt to rerun the sweep or change a setting. If the error persists, disconnect and restart the device.

> For any errors displayed on the SourceMeter display, refer to the SourceMeter Reference Manual at the top of the README document and submit them under the `Errors.txt` file of the repository


## License and Usage

This document, alongside its accompanying code and resources, originates from a faculty project undertaken at the Fakultät für Elektrotechnik und Informationstechnik on the Technische Universität Ilmenau. My aim in sharing this document is to highlight academic research, methodologies, and achievements.

### Academic Integrity:

Promoting open research and academic transparency, this work is shared with the larger community. However, it's vital to maintain academic integrity. If this work is utilized in academic publications or assignments, it should be referenced appropriately. Unauthorized reproduction without proper citation goes against our principles and is strongly discouraged.

For queries, collaborations, or any clarifications related to this project, please get in touch at: [elvap01@gmail.com].

### Contributors and Acknowledgments:

I wish to extend my gratitude and acknowledgment to the following individuals who played a significant role in this project:

- **prof. Bojana Petkovic**
  
- **prof. Frank Feldhoff**
  
- **prof. Hannes Töpfer**

Without their invaluable input, guidance, and contributions, this project wouldn't have reached its current form.
