### web application security by using Modsecurity
  Modsecurity  is an open-source web application firewall (WAF). Originally designed as a module for the Apache HTTP Server, it has evolved to provide an array of Hypertext Transfer Protocol request and response filtering capabilities along with other security features across a number of different platforms including Apache HTTP Server, Microsoft IIS and Nginx.
  Steps using modsecurity
  1. SHH in to the apache server (On Raseberrypie)
  2. Install modsecurity
     sudo apt install libapache2-mod-security2
     
     <img width="600" height="514" alt="Picture1installmod" src="https://github.com/user-attachments/assets/b1587bf6-4c0c-458a-a377-54b5ca82a840" />
  3. Move in to the mode security configuration file
     
     <img width="600" height="396" alt="Picture2cdmod" src="https://github.com/user-attachments/assets/04340f72-a589-481d-809e-e45eca559a21" />
  4. Before making any change, run version check
      apt-cache show libapache2-mod-security2
     
     <img width="1100" height="472" alt="Picture3version" src="https://github.com/user-attachments/assets/fb150d57-a4b7-447b-b531-8f920412f001" />
  5. initialization of mod-evasive
     sudo nano mod-security.conf
     
     <img width="480" height="247" alt="Picture5initialmodeivasive" src="https://github.com/user-attachments/assets/252985d8-716b-453d-bae1-59138ffe1ec6" />
     
     ### Two change were made
     
     **one- the option which specifies directives were commented out.**
     
     <img width="598" height="330" alt="Picture6changetoconf" src="https://github.com/user-attachments/assets/d8cc7dc8-2f6a-49ac-838d-945b9bf10f52" />
     
     Two - HTTP limits have parameter included LimitRequestBody, LimitRequestFields, LimitRequestField Size HTTP Limits allowed me to dictate the size of body requests (LimitBodyRequests), number of header fields accepted and the size of the header fields allowed from the user (LimitRequestField/LimitRequestFieldSize), the size of the HTTP request line accepted from the client (LimitRequestLine) and the size of an XML-based request body (LimitRequestBody).
In simpler terms, this gave me a method which would allow to control traffic to the server by restricting the number and size of requests made within a period of choosing. This is essential to ensuring fair use of services and to better help protect from DDoS attacks. But initially error was found due to syntax error

<img width="620" height="471" alt="Picture10error" src="https://github.com/user-attachments/assets/0746d376-9d9b-435c-b683-9b2309b3e72b" />

error check was run using, within the apache2 directory,  i run <i>apache2ctl configtest</i>

<img width="406" height="110" alt="Picture11errormessage" src="https://github.com/user-attachments/assets/7b90fb47-adb8-4d45-9c7e-354d246d2561" />

i understant what each http limits do and how do we set a limits to our server and provided the recommended numerical value for each limitrequest

<img width="620" height="318" alt="Picture8httplimist change" src="https://github.com/user-attachments/assets/133d3939-1654-4662-8f1d-ecd8d5d697a5" />

7. Run system check to determine the change have been accepted
   i run sudo systemtcl restart apache2
         sudo systemctl status apache2
   
  <img width="600" height="396" alt="Picture7restartserver" src="https://github.com/user-attachments/assets/5ecab8ae-3fec-4f69-9d43-4f7bb7f7eac2" />
  
  <img width="620" height="227" alt="Picture12testing" src="https://github.com/user-attachments/assets/fa5e7bc1-baff-43ff-8b9a-b6d13d5fb28d" />

 
