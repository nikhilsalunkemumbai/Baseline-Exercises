---
Title: Exercise 10: Analyze Process Memory Usage with VMMap
Objective: Learn to use Sysinternals VMMap to inspect a process's virtual and physical memory allocations, identifying various memory types like heap, stack, and mapped file usage.
Estimated Time: 30-60 minutes
Tools Needed:
*   Sysinternals VMMap (part of Sysinternals Suite) - Download from [Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/sysinternals-suite)
*   A target Windows process (e.g., Notepad.exe, Calculator.exe)
Setup Instructions:
1.  Download the Sysinternals Suite and extract it to a convenient location (e.g., `C:\Sysinternals`).
2.  Ensure you have administrative privileges on your Windows machine.
3.  Open a simple application like Notepad or Calculator. This will be our target process.
---

## Steps

1.  **Launch VMMap and Select a Process**
    *   Navigate to your Sysinternals Suite directory in File Explorer.
    *   Double-click `VMMap.exe` to launch the tool.
    *   In the "Select Or Launch Process" dialog box, go to the "View A Running Process" tab.
    *   Locate and select the `notepad.exe` (or `Calculator.exe`) process from the list and click "OK".
    *   *Observation:* Observe the main VMMap window, which displays graphical and tabular summaries of the process's memory usage.

2.  **Examine the Summary View**
    *   In the top section of the VMMap window, observe the three main bar graphs: "Committed", "Private Bytes", and "Working Set".
    *   Below these graphs, examine the "Summary View" table. This table categorizes memory allocations into types such as Image, Mapped File, Heap, Stack, Private Data, etc.
    *   *Question:* What are the total "Committed" bytes, "Private" bytes, and "Working Set" for your selected process? How do these values differ?

3.  **Explore Details View for Specific Memory Types**
    *   In the "Summary View" table, click on "Heap" to filter the "Details View" (lower pane) to show only Heap memory allocations.
    *   Observe the addresses, sizes, and protections of individual heap allocations.
    *   Repeat this for "Stack" and "Image" memory types. Note the differences in their characteristics (e.g., protection, shareability).

4.  **Capture and Compare Snapshots (Optional but Recommended)**
    *   While monitoring your process, perform some action in the target application (e.g., type some text in Notepad, perform a calculation in Calculator).
    *   In VMMap, go to `File` -> `Take Snapshot` (or press `F5`). This saves the current memory state.
    *   Perform more actions in the target application.
    *   Take another snapshot.
    *   Go to `File` -> `Compare Snapshots` and select your two snapshots. Observe the changes in memory allocations between the two states. Green indicates new allocations, red indicates freed allocations.

## Expected Output

(The actual output will vary depending on the chosen process and its activity. Below is a generic description.)

```
VMMap main window showing:
- Top bar graphs (Committed, Private Bytes, Working Set) with various levels.
- Summary View table listing memory types (Image, Mapped File, Heap, Stack, Private Data, etc.) with their respective committed and working set sizes.
- Details View (lower pane) showing individual memory regions (addresses, sizes, protection, details) for selected memory types.
- If snapshots are compared, differences highlighted in green (new) and red (freed).
```
![Expected output](images/exercise-10-output.png)

## Reflection Questions

1.  Based on your observations in the Summary View, what is the primary memory type consumed by the `notepad.exe` (or `Calculator.exe`) process? Why do you think this is the case?
2.  How could VMMap's snapshot comparison feature be useful in identifying a memory leak?
3.  What is the significance of the "Protection" column in the Details View when analyzing different memory regions? What might "Read/Write/Execute" protection indicate?

## Next Steps

*   [Exercise 11: Spot Suspicious Connections with TCPView](../sysinternals/11-spot-suspicious-connections.md)
*   Further reading: [VMMap documentation on Microsoft Learn](https://learn.microsoft.com/en-us/sysinternals/downloads/vmmap)

## Hints / Troubleshooting

*   **Administrative Privileges:** Ensure you run `VMMap.exe` with administrative privileges. Right-click `VMMap.exe` and select "Run as administrator" to correctly inspect all process memory details.
*   **Target Process Selection:** If your target process isn't listed, ensure it's running. If it's a very short-lived process, you might need to use the "Launch And Trace A New Process" tab in VMMap or other Sysinternals tools like ProcDump.
*   **Interpreting Colors:** In the graphical views, different colors represent different memory types (e.g., Heap, Stack, Private). Consult VMMap's legend for exact meanings if needed.
*   **Snapshots:** When taking snapshots for comparison, ensure there's a clear change in the application's state or memory usage between snapshots to see meaningful differences.
