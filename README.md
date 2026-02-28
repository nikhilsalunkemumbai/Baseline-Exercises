# Baseline-Exercises

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

"96 bite-size, hands-on exercises to master baseline technical skills."

## Mission Statement

To provide a free, structured path for anyone to gain practical, job-ready baseline technical skills through small, self-contained exercises that can be completed in a couple of hours. This project aims to democratize Baseline skills, emphasizing hands-on learning over theory, and building a community of practitioners who learn together.

---

### **⚠️ Disclaimer**
**This toolkit is provided for educational and administrative purposes only.** Automating security configurations or hardware management carries inherent risks. Always test scripts in a non-production environment before deployment. The authors are not responsible for any system downtime, data loss, or hardware issues resulting from the use of these tools.

---

## Project Goals

*   Break down complex Baseline domains into manageable, bite-size tasks.
*   Help learners build a portfolio of real-world work they can showcase.
*   Foster a collaborative learning community through shared exercises and solutions.
*   Encourage continuous improvement by keeping exercises up-to-date with evolving threats.

## Tone and Personality

This project embodies a professional, encouraging, and pragmatic approach to learning. It's technically precise, avoiding unnecessary jargon, yet supportive of learners at all levels, acknowledging that mistakes are an integral part of the learning process. Our focus is on practical, real-world applications that genuinely work in the field, rather than purely academic theory. We invite contributions, questions, and sharing, fostering a strong community spirit. Exercises are concise, with clear and direct instructions, ensuring an efficient learning experience.

## Why Bite-Size?

This project emphasizes the value of actionable, focused learning in enterprise infrastructure and Baseline consulting. Complex topics in cybersecurity can be daunting. By breaking down expertise into 1-2 hour exercises, this repository makes mastering baseline technical skills achievable. This format ensures minimal setup time, allowing practitioners and students to quickly dive into practical scenarios using free tools, local VMs, or sample data, and immediately produce tangible outputs for their portfolios.

## Who Is This For?

This repository is designed for:
*   **Primary Audience:** Security practitioners, students, and professionals who are looking to gain or transition into Baseline roles in foundational technical areas.
*   **Secondary Audience:** Recruiters and potential clients who seek to verify the owner's (and other contributors') practical skills through demonstrable work.

## Tools Covered

The exercises are structured around six core tools, each critical for a robust Baseline capability:

1.  **Wireshark**
    *   Focus: Networking / packet analysis.
    *   Description: Essential for deep inspection of network traffic to understand communications, diagnose issues, and detect anomalies.

2.  **Sysinternals Suite**
    *   Focus: Windows OS internals.
    *   Description: A collection of advanced utilities for managing, diagnosing, and troubleshooting Windows systems, crucial for understanding process activity, system calls, and security configurations. (Note: Exercises for this tool are Windows-only, leveraging extensive Windows OS knowledge.)

3.  **Nmap**
    *   Focus: Security fundamentals / network scanning.
    *   Description: A powerful tool for network discovery, security auditing, and port scanning, vital for mapping attack surfaces and identifying vulnerabilities.

4.  **Python**
    *   Focus: Scripting / automation for Incident Response.
    *   Description: A versatile programming language used for automating tasks, parsing logs, integrating with APIs, and developing custom tools for IR workflows.

5.  **Elastic Stack (ELK)**
    *   Focus: Logging & monitoring / SIEM.
    *   Description: A suite of open-source tools (Elasticsearch, Logstash, Kibana) for collecting, searching, analyzing, and visualizing log data, central to Security Information and Event Management (SIEM) operations.

6.  **NIST SP 800-61**
    *   Focus: Legal & regulatory framework.
    *   Description: Conceptual exercises focused on understanding and applying the NIST Special Publication 800-61 guidelines for Incident Response, producing critical documents, checklists, and plans.

## Getting Started

To get up and running with the `Baseline-Exercises` exercises quickly, follow these steps:

1.  **Clone the Repository:**
    ```bash
    git clone https://github.com/nikhilsalunkemumbai/Baseline-Exercises.git
    cd Baseline-Exercises
    ```
2.  **Set Up Your Environment**: Follow the detailed instructions in [`SETUP.md`](SETUP.md) to install all necessary tools and prepare your system.
3.  **Explore Exercises**: Dive into the exercises by browsing the [`exercises/README.md`](exercises/README.md) for a categorized list and learning paths.

## Exercise Workflow

Once your environment is set up, here's the recommended workflow for tackling the exercises:

1.  **Choose an Exercise:** Select an exercise from the [`exercises/README.md`](exercises/README.md) that aligns with your learning goals.
2.  **Follow Instructions:** Each exercise markdown file (`.md`) contains detailed objectives, tool-specific setup, step-by-step guides, and reflection questions.
3.  **Perform Hands-on Work:** Execute the steps in your lab environment (local VM, cloud sandbox, etc.).
4.  **Document Your Output:** Capture screenshots, script outputs, or report snippets as evidence of your work.
5.  **Reflect and Learn:** Answer the reflection questions to deepen your understanding.
6.  **Troubleshooting**: If you encounter any issues, refer to the [`TROUBLESHOOTING.md`](TROUBLESHOOTING.md) for common solutions.
7.  **Share Your Results (Optional):** Consider creating a personal portfolio or blog post about your completed exercises.

## Prerequisites

*   Basic IT and networking knowledge.
*   A computer with internet access.
*   Administrative privileges on your local machine.
*   Familiarity with using a command-line interface.
*   (Optional but Recommended for Sysinternals exercises) A Windows Virtual Machine (e.g., using VirtualBox, VMware).

## Repository Structure

The project is organized for easy navigation and extensibility:

*   `.` (Root Directory)
    *   `README.md`: This file, serving as the project's main introduction.
    *   `LICENSE`: Details the licensing terms for the project (MIT License).
    *   `CONTRIBUTING.md`: Guidelines for community contributions.
    *   `CODE_OF_CONDUCT.md`: Standard contributor behavior guidelines.
    *   `/exercises`: Contains all hands-on exercises, categorized by tool.
        *   `/exercises/wireshark`: Wireshark-specific exercises.
        *   `/exercises/sysinternals`: Sysinternals Suite-specific exercises.
        *   `/exercises/nmap`: Nmap-specific exercises.
        *   `/exercises/python`: Python scripting exercises.
        *   `/exercises/elastic-stack`: Elastic Stack (ELK) exercises.
        *   `/exercises/nist-sp-800-61`: NIST SP 800-61 conceptual exercises.
            *   Each tool subfolder contains `01-exercise-name.md` through `16-exercise-name.md` files.
    *   `/resources`: An optional directory for shared materials (e.g., sample PCAPs, log files, scripts). Large files are linked externally.
    *   `/solutions`: An optional directory for example solutions, mirroring the `/exercises` structure. (Learners are encouraged to attempt exercises before consulting solutions.)
    *   `/templates`: Contains templates for new exercises, facilitating consistent formatting for contributors.
    *   `/docs`: Additional documentation, such as setup guides for common lab environments.

## Get Involved

We welcome contributions! Whether you want to propose new exercises, suggest improvements, fix errors, or add translations, your input helps strengthen this community resource. Please refer to `CONTRIBUTING.md` for detailed guidelines.

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for full details.

## Acknowledgments

Special thanks to the creators and maintainers of Wireshark, Sysinternals Suite, Nmap, Python, Elastic Stack, and NIST for providing invaluable tools and frameworks that make this project possible.

## Contact / Connect

Connect with the project owner on [LinkedIn](www.linkedin.com/in/nikhilsalunke45) for questions, feedback, or collaborations.