# DeepLabCut Tutorial

**TODO: Divide this up into separate files or at least organize it better**

## DeepLabCut Usage **TODO**

### HPCC Access **TODO**

### Starting Up DeepLabCut Software **TODO**


## DeepLabCut Installation on HPCC

1. Install Anaconda **TODO**

2. Create virtual environment

	`conda create -n DLC python=3.8`<br>
	`conda activate DLC`
	
3. Install DeepLabCut

	`pip install deeplabcut`

4. Load HPCC modules

	`module load cuDNN/7.6.4.38-CUDA-10.1.105`<br>
	`module load FFmpeg`
	
5. Install correct versions of Tensorflow and Keras **TODO**
	
6. Start Ipython & import DeepLabCut

	`ipython`<br>
	`import deeplabcut`
	
7. Clone DeepLabCut Repository & Run Test Script

	1. Navigate to directory you would like to save the DeepLabCut 
	   source code
	   
	2. Clone the repo & navigate to test script directory
	
		`git clone https://github.com/DeepLabCut/DeepLabCut.git`<br>
		`cd DeepLabCut/examples`
		
	3. Run test script - **the test script will produce a lot of output, but at the end you should see "ALL DONE!!! - default cases are functional." near the bottom**
	
		`python testscript.py`
	   
	   
## Installing & Configuring Annotation Software on Windows

1. Download Python 3.9 from the Windows Store

2. Open PowerShell

3. Create a directory for your project

	`mkdir my_project`<br>
	`cd my_project`
	
4. Install Label Studio - **This may take a long time**

	`pip install label-studio`
	
5. Add Label Studio executable ptath to user environment variables **TODO**

6. Create a Label Studio Account

7. Create and Configure Project **TODO: link to google drive**

	- Download the file in Google Drive "Hens_Collab/DeepLabCutDocumentation/LabelStudio/label-studio-config.xml

	- Go to the settings for your new project
	
	- Go to "Labeling Interface"
	
	- Click on "Code"
	
	- Copy and paste the contents of the label-studio-config.xml file into the box
	
	- Click save
	
	- Now, once you import your data, you can begin labeling. When labeling
	  the first hen, for example, type "hen1-" in the filter box at the top 
	  and it will hide the labels you don't care about.
	  

## Bodypoints **TODO: link to google drive**

I recommend reviewing my examples in Google Drive under "Hens_Collab/DeepLabCutDocumentation/Labeling Examples/"

| Bodypart  | Description |
|-----------|-------------------------------------------------------------- |
| Beak 		| Tip of beak, if it's not visible, make your best guess |
| Comb		| In the middle of the comb, close to the hen's head |
| Blade		| Back of the comb, close to the hen's head |
| Hackle 1	| Just behind the blade, as close as you can get it to the blade without touching it |
| Hackle 2	| Halfway between Hackle 1 & 2, account for the orientation of the hen's neck |
| Hackle 3	| Where the neck anchors to the body |
| Spine 1 	| Just behind hackle 3, as close as you can get it to hackle 3 without touching it |
| Spine 2	| One third of the distance between Spine 1 & 4 |
| Spine 3	| Two thirds of the distance between Spine 1 & 4 |
| Spine 4	| The final point on the hen's back that is not the tail |
| Tail 1	| First point on tail, this should be very close to spine 4 |
| Tail 2	| Halfway between tail 1 & 3, account for tail  orientation |
| Tail 3	| Last point on tail, if feathers split apart a	t the end, use last visible point in center |


## Using DeepLabCut on the HPCC

### Setup

1. Upload your videos to the HPCC

2. To start (anytime you use DeepLabCut), enter the following in the terminal
	
	`module load cuDNN/7.6.4.38-CUDA-10.1.105`<br>
	`module load FFmpeg`<br>
	`ipython`<br>
	`import deeplabcut`
	
2. Create a new project
	
	- You will need the config_path for most DeepLabCut commands, so it is useful to
	  save it at the  beginning of a work session. It is a string containing the path to the config.yaml
	  file in your DeepLabCut project. The create_new_project (you only run this once to create a project)
	  function will output the path to the config file. Reference the "DeepLabCut Usage" section of this
	  document to see how to save the config_path in normal use cases.
	
	    `config_path = deeplabcut.create_new_project('HenTracks','YourName', multianimal=True)`
              
3. Configure the project **TODO: link to google drive**
    
    1. Download configuration file from Google Drive: Hens_Collab/DeepLabCutDocumentation/config.yaml
     
    2. Replace the config.yaml in your project folder with the new file
     
    3. Open the config.yaml file in a text editor
     
    	1. Change "scorer" to your initials
    	2. Change "project_path" to your DeepLabCut project path
    	3. Change the paths for each video to the correct path for your HPCC account
    	4. Optionally: Add a skeleton definition, see docs for details. This skeleton will be used when
    	   plotting analysis, however a data-driven skeleton will still be used for tracking & assembly
    	   
   4. See docs for other configuration options
   	 
### Export Label Studio Labels **TODO: link to google drive**
	
1. From your hens project main page in label studio, click Export -> JSON -> Export
2. Save the JSON file
3. Download the python script from Google Drive: Hens_Collab/DeepLabCutDocumentation/LabelStudio/cvt-label-studio-to-DLC.py
4. Upload the JSON file and the python script to the HPCC
5. If you are still in your ipython terminal, type "exit()" and hit the Enter key
6. Enter the following in the terminal in the directory with the python script
	
    `python cvt-label-studio-to-DLC.py PATH-TO-YOUR-JSON-FILE PATH-TO-YOUR-DEEPLABCUT-PROJECT`
	
7. This will place a csv file with the labels for each frame in the appropriate folder in your DeepLabCut project
8. Register labels with DeepLabCut
		
	- In your HPCC terminal
		
		`cd YOUR-DEEPLABCUT-PROJECT-DIRECTORY`<br>
		`module load cuDNN/7.6.4.38-CUDA-10.1.105`<br>
		`module load FFmpeg`<br>
		`ipython`<br>
		`import deeplabcut`<br>
		`import os`<br>
		`config_path = os.path.join(os.getcwd(), 'config.yaml')`<br>
		`deeplabcut.convertcsv2h5(config_path, userfeedback=False)`
		
9. From here, you should be able to follow the [User Guide](https://github.com/DeepLabCut/DeepLabCut/blob/master/docs/maDLC_UserGuide.md) starting from the heading "Create Training Dataset
