# DShield-Honeypot
A Raspberry Pi-based honeypot designed to monitor and log suspicious network activity. The honeypot logs data and sends it to the DShield dashboard for analysis.

# How It's Built
**Technologies Used:** Raspberry Pi, DShield, OpenSSL, SSH, Nmap, DShield API

This project utilizes a Raspberry Pi running a honeypot to capture potential network intrusions. The honeypot is connected to DShield, which collects and analyzes network traffic for malicious activity. The setup includes installing DShield on the Raspberry Pi, generating SSL certificates for decoy purposes, and exposing the honeypot to the internet for real-time monitoring. Tools like Nmap are used to scan the system, and logs are forwarded to the DShield dashboard using the DShield API.

Key steps:
- Installed Raspberry Pi OS Lite and configured SSH for remote access.
- Used OpenSSL to generate SSL certificates, creating a decoy for attackers.
- Cloned the DShield repository to set up the honeypot.
- Registered the honeypot with DShield by fetching an API key from the DShield website.
- Integrated the DShield API to ensure continuous log forwarding to the dashboard.
- Used the API to monitor network activity and view logs on the DShield dashboard.
- Monitored the honeypot in real time using various Nmap scans to gather logs and insights.

# API Usage
The DShield API plays a crucial role in this project, facilitating real-time communication between the honeypot and the DShield servers. After registering the honeypot, the API key was used to authenticate the honeypot with the DShield platform. Logs and data collected by the honeypot are sent to the DShield dashboard using the API, where they are analyzed for potential attacks and vulnerabilities.

By using the API, the honeypot:
- Sends live logs and data to the DShield platform for continuous monitoring.
- Receives feedback on network activity, helping to analyze suspicious traffic.
- Tracks and visualizes attacks through the DShield dashboard in real time.

# Optimizations
Various measures were implemented to ensure the honeypot could efficiently handle traffic:
- Generated SSL certificates to deceive attackers into thinking they were interacting with a secure service.
- Used the aggressive Nmap scan settings (e.g., `-T5` and `-p-`) to identify open ports and vulnerabilities, while also testing the response of the honeypot.
- Streamlined the Raspberry Pi setup for minimal resource usage, allowing it to run continuously without overheating or lagging.

# Lessons Learned
Security Trade-offs: In creating this honeypot, I learned how to balance the exposure of vulnerabilities with the need for securing critical parts of the system. Setting up fake SSL certificates made the honeypot appear more legitimate, but also attracted more aggressive attacks.

Networking Insights: Setting up a honeypot allowed for a deeper understanding of networking protocols and attack vectors. Watching the real-time logs provided insights into how attackers probe for weaknesses.

API Integration: Using the DShield API helped streamline the process of log forwarding and analyzing incoming network data. It highlighted the importance of understanding APIs for real-time data processing and remote monitoring.

# Acknowledgements
Special thanks to the DShield project for their open-source resources and documentation that made setting up the honeypot seamless. Thanks also to the creators of Nmap and OpenSSL, whose tools were instrumental in this project. The API integration would not have been possible without the extensive DShield API documentation.
