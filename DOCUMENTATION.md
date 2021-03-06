# How the program works (in theory :eyes:)
1. Access the moodle website in a headless browser
    1. Login to MyConcordia by accessing the USERNAME and PASSWORD variables in .env
    2. Fetch the course links for moodle hidden in the HTML of MyConcordia's home page
    3. Open each individual page in the headless browser
2. Fetch the contents of the website periodically and compare them to past versions if there are any
    1. Fetch the HTML content
    2. Parse it and store an array of *documents* in currentFiles.txt **if** the currentFiles.txt is empty
    3. Compare the current HTML content's *documents* to currentFiles.txt's documents
        - If it's the same, do nothing
        - If it's different, find the differences and send the array of differences to the renderer process
        - The renderer process displays it
# :file_folder: Structure
```
Root folder
├── Frontend
│   ├── aww.png
│   ├── bunny_with_hat.jpg
│   ├── index.html
│   ├── index.js
│   └── styles.css
├── src
│   ├── util
│   │   ├── Constants.js
│   │   └── FileFunctions.js
│   └── script.js
├── DOCUMENTATION.md
├── .gitignore
├── package.json
├── package-lock.json
├── defaultNotificationSound.mp3
└── README.md


platformSpecificPath/Electron/
├── currentFiles.txt
├── data.txt
├── configuration.txt
├── .env
└── notifications.txt
```
### Miscellaneous files
- .git keeps track of the repository
- .gitignore allows us to set what we don't want to be tracked
- README.md is the markdown file explaining the purpose of the program and how to install/run it
- DOCUMENTATION.md is this file, it's just a way to keep track of what's what as the program gets larger
- package.json holds metadata for the program - if you want to run the program you need it. It holds the commands like `npm run default` and `npm run make-linux` - all the make configuration and dependencies are also stored here
- node_modules/ are the external modules/libraries that the program requires to run - these modules can be installed with `npm install` when in the presence of the package.json
- package-lock.json frankly don't know what this is for...
- defaultNotificationSound.mp3 is the default notification sound

### Frontend files
- index.js contains the renderer process' logic 
- index.html contains the HTML for the frontend
- styles.css contains the CSS for the frontend
- .png files here are the icons

### src files
- script.js is the main process. It is through the main process that the headless browser is launched and files are fetched and compared
- util/ contains utility files to avoid redundancy:
    - Constants.js which is just a bunch of constants used throughout the program
    - FileFunctions.js contains all the functions which relate to file reading and writing. This code can be reused elsewhere if need be.

### Logs and .env
These files are located in an OS dependant path + moodle-notifier/
The OS dependant path are:
- `%APPDATA%` on Windows
- `$XDG_CONFIG_HOME` or `~/.config` on Linux
- `~/Library/Application Support` on macOS

For instance, for Linux - these files could be in `~/.config/moodle-notifier/`

- currentFiles.txt which is used to compare the previous files and the current one, even between program shutdowns
- data.txt which contains a list of the 'states' of currentFiles.txt where one comparison had a difference with the next. That is to say, if I refresh the page and it's the same, nothing gets added to data.txt, but if a file was removed or added, the new state will be appended to data.txt. This is in case you want to see the history of changes.
- notifications.txt contains the list of notifications displayed. It is used to allow the user to press 'CTRL+Z' to go back to a previous state, and it gets modified when the user modifies the list, for instance, by dismissing a notification. There is a notification limit which essentially limits the amount of notifications that the user wants stored for 'CTRL+Z' functionality
- configuration.txt is the configuration file for this entire program. In essence it's a JSON containing key value pairs, such as 'audioFilePath:"path/to/audio/file.mp3". It exists so that you can make modifications and they will stick between shutdowns.

### Uninstalling

###### Debian/Ubuntu:
`$ sudo apt-get remove moodle-notifier`

###### Windows:
Control panel -> Programs and features -> Add/Remove programs -> Find moodle-notifier and click uninstall
