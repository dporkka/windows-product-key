# windows-product-key
To display your current Windows product key using the Command Prompt or PowerShell, follow these steps:

### Using Command Prompt

1.  Open Command Prompt as Administrator:

    -   Click on the Start menu.
    -   Type `cmd` or `Command Prompt`.
    -   Right-click on Command Prompt and select `Run as administrator`.

2.  Run the Following Command:

    `wmic path SoftwareLicensingService get OA3xOriginalProductKey`

    This command retrieves the product key embedded in your system's BIOS/UEFI firmware.

### Using PowerShell

1.  Open PowerShell as Administrator:

    -   Click on the Start menu.
    -   Type `PowerShell`.
    -   Right-click on Windows PowerShell and select `Run as administrator`.

2.  Run the Following Command:

    `powershell "(Get-WmiObject -query 'select * from SoftwareLicensingService').OA3xOriginalProductKey"`

    This command also retrieves the product key from your system's BIOS/UEFI firmware.

### Notes:

-   OEM Keys: These commands typically retrieve the OEM product key embedded in the BIOS/UEFI. If you have a retail product key that you manually entered, it might not be retrieved by these commands.
-   Security: Be cautious when handling your product key. Avoid sharing it publicly or storing it in an insecure location.

If the product key isn't being displayed using the previous methods, it may be because the product key is not stored in the BIOS/UEFI firmware or that the command didn't work as expected. Here are alternative methods to retrieve your Windows product key:

### Using PowerShell Script

You can use a PowerShell script to retrieve the product key from the Windows Registry:

1.  Open PowerShell as Administrator:

    -   Click on the Start menu.
    -   Type `PowerShell`.
    -   Right-click on Windows PowerShell and select `Run as administrator`.

2.  Run the Following PowerShell Script:
   
function Get-WindowsKey {

    $hklm = [UInt32] "0x80000002"

    $path = "Software\Microsoft\Windows NT\CurrentVersion"

    $name = "DigitalProductId4"

    $wmi = [WMIClass]"\\.\root\default:stdRegProv"

    $data = $wmi.GetBinaryValue($hklm, $path, $name)

    $key = ($data.uValue)[52..66]

    $productkey = ""

    for ($i = 24; $i -ge 0; $i--) {

        $current = 0

        for ($j = 14; $j -ge 0; $j--) {

            $current = $current * 256 -bxor $key[$j]

            $key[$j] = [math]::Floor($current / 24)

            $current = $current % 24

        }

        $productkey = ("BCDFGHJKMPQRTVWXY2346789"[$current]) + $productkey

        if (($i % 5) -eq 0 -and $i -ne 0) {

            $productkey = "-" + $productkey

        }

    }

    return $productkey

}

Get-WindowsKey


### Windows Edition Differences:

-   Cloud Edition: Lightweight, cloud-focused.
-   Professional Education: Education-specific features for teachers and students.
-   Professional Workstation: High performance for demanding workloads.
-   Education: Education-focused with tools for safe and productive learning.
-   Professional Country Specific: Tailored for compliance with local regulations.
-   Professional Single Language: Single language support.
-   ServerRdsh: Optimized for Remote Desktop Services.
-   IoT Enterprise: Designed for IoT devices with enterprise-level management.
-   Enterprise: Advanced features for large organizations with high security and manageability needs.

To upgrade to the "Professional Workstation" edition from the terminal using the DISM (Deployment Image Servicing and Management) tool, follow these steps:

1.  Open Command Prompt as Administrator: Make sure you open Command Prompt with administrative privileges.

2.  Use the DISM Command to Upgrade: Use the `DISM /Set-Edition` command to upgrade your Windows edition. You will need the product key for the Professional Workstation edition.

Here's how to do it:

Copy this code below and replace `XXXXX-XXXXX-XXXXX-XXXXX-XXXXX` with your product key.

`DISM /Online /Set-Edition:ProfessionalWorkstation /ProductKey:XXXXX-XXXXX-XXXXX-XXXXX-XXXXX /AcceptEula`


## Troubleshooting Tips

If the above steps didn't work, here is an alternative approach using the `slmgr.vbs` script to change your Windows edition:

### Using slmgr.vbs Script

1.  Open Command Prompt as Administrator:

    -   Click on the Start menu.
    -   Type `cmd` or `Command Prompt`.
    -   Right-click on Command Prompt and select `Run as administrator`.
2.  Use the slmgr.vbs Command to Enter the New Product Key:

    -   Enter the following command, replacing `XXXXX-XXXXX-XXXXX-XXXXX-XXXXX` with your actual product key for the Professional Workstation edition:

        Copy this code below after adding your Product Key.

        `slmgr.vbs /ipk XXXXX-XXXXX-XXXXX-XXXXX-XXXXX`

3.  Activate the Product Key:

    -   After entering the product key, run the following command to activate it:

        `slmgr.vbs /ato`

### Alternative: Changing Product Key via Settings

If the command-line approach doesn't work, you can also change the product key through the Windows Settings:

1.  Open Settings:

    -   Press `Windows + I` to open the Settings app.
2.  Navigate to Update & Security:

    -   Click on `Update & Security`.
3.  Go to Activation:

    -   In the left-hand menu, click on `Activation`.
4.  Change the Product Key:

    -   Click on `Change product key` and enter the new product key for the Professional Workstation edition.

### Ensure You Have the Correct Edition

Make sure the product key you are using corresponds to the edition you want to upgrade to. If the product key is incorrect or for a different edition, the upgrade will fail.

### Checking Logs for More Details

If you continue to face issues, reviewing the DISM log file for more detailed error information can be helpful:

-   Navigate to `C:\WINDOWS\Logs\DISM\dism.log`.
-   Open the log file with a text editor to see if there are more specific details about why the operation failed.

These steps should help you successfully change your Windows edition to Professional Workstation.
