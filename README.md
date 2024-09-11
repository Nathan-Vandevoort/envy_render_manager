# ENVY

Envy is a render manager with native Maya and Houdini support. Envy was built and designed for the Gnomon campus and that use case has influenced the design a great deal. As Envy was built for Gnomon students this readme will assume you are one.

## Table Of Contents
- [Features](#Features)
- [Installation](#Installation)
- [Usage](#Usage)
  - [Basics](#Basics)  
  - [Maya](#Maya)
  - [Houdini](#Houdini)


## Features
- First class maya support: via the envy plugin
- First class Houdini support: via custom built HDAs
- User customization: each envy user is able to completely customize their instance's functionality. 
- Plugin support: for users to support any softwares in the future.
- Render node failover: If a render node is signed out the scheduler will simply reallocate the work to another node.
- Server failover: If the machine running the Envy server is signed out the remaining nodes will elect a new server.
- Management GUI: To allow users to graphically manage their work items.
- Automatic signout: To allow Gnomon students to take as many render nodes as needed without worrying about signing out of them in the morning.


## Installation
Installation is very simple!
1. Navigate to `\\titansrv\studentShare\__ENVY__\`
2. Copy the `envy` folder to your Z:/ drive
3. It's **VERY IMPORTANT** that the envy folder is at the root of your zdrive. It should look like `Z:/envy`


## Usage
Using envy is also very simple. The following section will be split in usage for Maya and Houdini feel free to skip to which ever section you require.

### Basics
Your `Z:/envy` folder should look kind of like this:

![envy_layout](https://github.com/user-attachments/assets/def6542b-82ef-484f-aa61-b51399753f53)

The launch_envy.py file is what you open on every computer you want to render on. Any computer running envy will sign out at 8:30 am by default.

The launch_console.pyw file is what you open to monitor envy and cancel jobs. Here's a walk through of the console:

![envy_console_layout](https://github.com/user-attachments/assets/30fb5e3c-64f4-4dd7-b867-81f3cd5dbc79)

The Large window on the left is the viewport. Each one of the little blue balls represents a computer you have signed in. The top right window is your job viewer. You can see I have an in progress job called test_scene.0001. In the bottom right window is your console. Feel free to mess around with it! I'll talk more about the console in the [Customization](#Customization) section. If you ever want to cancel a job simply right click the job and mark it as finished.


### Maya

To setup the maya plugin for envy need to type `install_maya_plugin()` into the console and press enter. 

**Please Note:** This command will only install the maya plugin if you have already opened maya this term. If you use a new version of maya during the term you will need to type in `install_maya_plugin()` again.

The console should inform you that the maya plugin was installed successfully. To use the plugin within maya open the plugin manager under `windows -> settings/preferences -> plugin manager` and then check `load` on the envy plugin.

![maya plugin manager](https://github.com/user-attachments/assets/0ced8e6f-ce27-46c6-84de-7484288df233)

You should see an envy menu appear at the top of maya:

![envy dropdown](https://github.com/user-attachments/assets/cdb60104-68e3-4904-bdde-4786a2f0b1f8)

If you click that dropdown and select `Export to Envy` another UI window should appear and it should look like this:

![mayaUI-ezgif com-optimize](https://github.com/user-attachments/assets/5d959639-5b5d-4078-b62d-38069987d7be)


Right now you can see I have two cameras which are both inactive. To tell envy to render this camera you simply click on the camera icon and it should turn blue. You can also click on the clock icon to adjust the frame range per camera to have envy render your sequences for you. the `Start Frame` and `End Frame` parameters set themselves from your render settings but you can overwrite them within this UI. the `Batch Size` parameter is how many frames will be allocated to each computer, learn more about batch sizes and what they mean practically [here](#batch-size)!. 



## Requirements
Envy was built for the Gnomon campus and such has no gaurentee to work in other environments without some customization. Here are the major requirements:
- Some sort of network attached storage drive/partition which is unique to each user
- Some sort of network attached storage which all users have access to
- User's machines must be running Windows
- At least Python 3.10 on User's machines
- All User machines must be on the same LAN


## Resources
I created a discord servers for users to get in touch with me and report bugs. Feel free to [join](https://discord.gg/r259susGAS)!

## Glossary
### batch size
To put it simply the batch size is how many frames will envy try to give each computer. You may want to increase this number if you have very fast renders (less then 2 minutes per frame). Under the hood envy needs to launch whatever software you want to render in (maya, houdini, etc...) and that can take time. The batch size tells envy how often should each computer restart that software. If it takes longer to open maya then it does to render your frame then you probably want to increase your batch size. The batchsize also tells envy how many work items the scheduler can allocate. which is `(frame range // batch size)`.


For Example: if you have 100 frames to render and a batch size of 10 then 10 computers will be able to work on your job. This can be not optimal if you have 20 computers running envy as 10 of them wont have any work. 
In this case you may want to set a lower batchsize such as 5. If you dont want to worry about batch size just leave it at 1.
