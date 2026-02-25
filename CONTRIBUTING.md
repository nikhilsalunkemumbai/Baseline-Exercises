# Contributing to Baseline-Exercises

We welcome and appreciate your contributions to the Baseline-Exercises project! Whether you're fixing a bug, adding a new exercise, improving existing content, or updating documentation, your help is valuable. This document outlines the guidelines for contributing to ensure a smooth and collaborative process.

## How to Contribute

1.  **Fork the Repository:** Start by forking the `Baseline-Exercises` repository to your GitHub account.
2.  **Clone Your Fork:** Clone your forked repository to your local machine:
    ```bash
    git clone https://github.com/nikhilsalunkemumbai/Baseline-Exercises.git
    cd Baseline-Exercises
    ```
3.  **Create a New Branch:** Create a new branch for your changes. Use a descriptive name that reflects the nature of your contribution (e.g., `feature/new-wireshark-exercise`, `fix/nmap-typo`, `docs/update-setup-guide`).
    ```bash
    git checkout -b your-branch-name
    ```
4.  **Make Your Changes:** Implement your changes, adhering to the project's conventions and standards.
    *   **New Exercises:**
        *   Follow the structure outlined in `templates/exercise-template.md`.
        *   Place new exercises in the appropriate tool subfolder within `/exercises`.
        *   Use the naming convention `XX-descriptive-name.md` (e.g., `01-capture-http-request.md`). Ensure the two-digit number is sequential within its tool folder.
        *   Provide clear, concise, and technically accurate instructions.
        *   Suggest external links for sample data if large files are needed.
        *   Include screenshot placeholders `![Expected output](images/exercise-XX-output.png)` where relevant.
    *   **Improvements/Bug Fixes:** Clearly describe the issue you're addressing and how your changes resolve it.
    *   **Documentation:** Ensure all documentation is up-to-date, clear, and consistent with the project's tone.
5.  **Test Your Changes:** If applicable, test your changes thoroughly to ensure they work as expected and do not introduce new issues.
6.  **Commit Your Changes:** Write clear and concise commit messages. A good commit message explains *what* changed and *why*.
    ```bash
    git commit -m "feat: Add new Wireshark exercise on HTTP filtering"
    ```
7.  **Push to Your Fork:** Push your local branch to your forked repository on GitHub.
    ```bash
    git push origin your-branch-name
    ```
8.  **Create a Pull Request (PR):**
    *   Go to the original `Baseline-Exercises` repository on GitHub.
    *   You should see a prompt to create a new pull request from your recently pushed branch.
    *   Provide a detailed description of your changes in the PR, including:
        *   What problem does this PR solve?
        *   How does it solve the problem?
        *   Any relevant context, design decisions, or trade-offs.
        *   If it's a new exercise, briefly describe its objective and expected learning outcome.
    *   Link to any relevant issues (e.g., `Fixes #123`).

## Code Standards and Conventions

*   **Markdown Formatting:** Use GitHub-flavored Markdown for all `.md` files.
*   **Language:** Maintain a professional, encouraging, and pragmatic tone. Avoid overly academic language or unnecessary fluff.
*   **Conciseness:** Keep instructions and descriptions to the point.
*   **Technical Accuracy:** Ensure all technical details, commands, and explanations are correct.
*   **File Naming:** Adhere to the `XX-descriptive-name.md` convention for exercises.
*   **Consistency:** Maintain consistency in terminology, formatting, and structure across all files.

## Seeking Help

If you have questions, need clarification, or are unsure about anything, feel free to:
*   Open an issue in the repository.
*   Reach out to the project maintainers via their contact information in `README.md`.

Thank you for helping to build a valuable resource for the cybersecurity community!