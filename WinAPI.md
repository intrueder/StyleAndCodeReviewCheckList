1. Use `HANDLE_MSG` macro instead of huge `switch..case` block

2. Use `ZeroMemory` instead of `memset` with `0` as fill pattern
   ```c
   // bad
   memset(buffer, 0, sizeof(buffer));
   
   // good
   ZeroMemory(buffer, sizeof(buffer));
   ```

3. Use helper macro from `WindowsX.h` instead of `SendMessage`
   ```c
   // bad
   SendMessage(hCtrl,LB_RESETCONTENT,0,0L);
   
   // good
   ListBox_ResetContent(hCtrl);
   ```

4. Use definitions 
   ```c
   // bad
   ShowWindow(hDlg, 1);
   
   // good
   ShowWindow(hDlg, SW_SHOW);
