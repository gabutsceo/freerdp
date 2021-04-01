name: RDP-VPS

on: workflow_dispatch

jobs:
  build:

    runs-on: windows-latest
    timeout-minutes: 9999

    steps:
    - name: Download Ngrok & NSSM
      run: |
        Invoke-WebRequest https://drive.google.com/uc?id=1MmvM4nbk5VDP5Mb9T4L9_sexY4jt4SA6  -OutFile ngrok.exe
        Invoke-WebRequest https://drive.google.com/uc?id=1qjSqMi9ptMi6-zQ3I6P7Ze1mH74n0-VU  -OutFile nssm.exe
    - name: Copy NSSM & Ngrok to Windows Directory.
      run: | 
        copy nssm.exe C:\Windows\System32
        copy ngrok.exe C:\Windows\System32
    - name: Connect your NGROK account
      run: .\ngrok.exe authtoken $Env:NGROK_AUTH_TOKEN
      env:
        NGROK_AUTH_TOKEN: ${{ secrets.NGROK_AUTH_TOKEN }}
    - name: Download Important Files.
      run: |
        Invoke-WebRequest https://drive.google.com/uc?id=1Lzc8kMmVCOTJdwmPp_q0yIYykOQ7-UQ5  -OutFile NGROK-AP.bat
        Invoke-WebRequest https://drive.google.com/uc?id=1S1DNawvQAV0AMZ4xXGAydxFspG7Lb1bs  -OutFile NGROK-CHECK.bat
        Invoke-WebRequest https://drive.google.com/uc?id=1w2gP_rkj4vRSq5kGiwARYhaAUMHWEy9G  -OutFile loop.bat
    - name: Make YML file for NGROK.
      run: start NGROK-AP.bat
    - name: Enable RDP Access.
      run: | 
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server'-name "fDenyTSConnections" -Value 0
        Enable-NetFirewallRule -DisplayGroup "Remote Desktop"
        Set-ItemProperty -Path 'HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp' -name "UserAuthentication" -Value 1
    - name: Create Tunnel
      run: sc start ngrok
    - name: Connect to your RDP CPU 2 Core - 7GB Ram - 255 SSD.
      run: cmd /c NGROK-CHECK.bat
    - name: All Done! You can close Tab now! Maximum VM time:6h.
      run: cmd /c loop.bat
