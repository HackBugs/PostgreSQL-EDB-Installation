## Putty auto login

1. `Putty.exe` location
2. Make `multiple shortcut` of PuTTY
3. Save all shortcut with your ip adress as you understand
4. Putty > Properties > shortcut > target > paste code here...

```
"C:\Program Files\PuTTY\putty.exe" username@192.168.37.142 -pw Password 
"C:\Program Files\PuTTY\putty.exe" username@192.168.33.148 -pw Password 
"C:\Program Files\PuTTY\putty.exe" username@192.168.33.149 -pw Password 
"C:\Program Files\PuTTY\putty.exe" username@192.168.41.26 -pw Password 
```

## Hide folder 📁 on windows 

1 Open Drive and go on path of folders location - on search bar write cmd

<!-- check hidden folders -->
<!-- folder_name hide folder cmd-->

```
Dir /a 
attrib +h +r +s # For hide folder
attrib -h -r -s # For show folder 
```
