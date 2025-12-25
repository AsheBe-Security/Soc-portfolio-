# Web-server (Apache)security
This project focused on apache web server security hardening using the techniques of 
  ### hiding the server banner 
  - Defualt apache server setting reveal too much information (sever type, OS ip address and open port)
    <img width="379" height="257" alt="server banner" src="https://github.com/user-attachments/assets/5765aa1d-6666-4d55-82d3-124251e0e663" />

  - Check if the service we looking for is up and running
    
    <img width="585" height="435" alt="Picture1" src="https://github.com/user-attachments/assets/43109da4-d304-4ec1-9aff-20911640b272" />

  - SSH in the Rasberry pi server where apache2 web server is runnig
    <img width="1160" height="552" alt="Picture3" src="https://github.com/user-attachments/assets/b7093bac-8889-4b2d-8ea7-8cf8e945e88e" />
    
  - access the configuration file by moving in to etc direcory
     Cd /etc/apache2/
     **sudo nano apache2.conf**
    <img width="1169" height="656" alt="Picture5" src="https://github.com/user-attachments/assets/0bb6951d-d5cd-40a6-9dea-8dea8e0889ef" />
    <img width="1108" height="646" alt="Picture4" src="https://github.com/user-attachments/assets/b6c4b99f-6d12-423a-a77e-eac4fc70b019" />
    <img width="1013" height="580" alt="Picture4" src="https://github.com/user-attachments/assets/994f7cf5-8e26-4f2a-8712-58f827df073f" />
    
   - Confirmation (run error on the url to see the server response (all attack vector information have been removed)
     <img width="406" height="286" alt="Picture55" src="https://github.com/user-attachments/assets/de2ca7a2-546c-4d88-868f-a32f011036c0" />
