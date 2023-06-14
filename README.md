# printme Prints all documents in a folde
Add-Type -AssemblyName System.Windows.Forms
[System.Windows.Forms.Application]::EnableVisualStyles()

$Form = New-Object System.Windows.Forms.Form
$Form.AutoSize = $true
$Form.Text = "Form"
$Form.TopMost = $false

$TextBox1 = New-Object System.Windows.Forms.TextBox
$TextBox1.Multiline = $false
$TextBox1.Width = 300
$TextBox1.Height = 20
$TextBox1.Location = New-Object System.Drawing.Point(50, 50)
$TextBox1.Font = New-Object System.Drawing.Font('Microsoft Sans Serif', 10)

$Label1 = New-Object System.Windows.Forms.Label
$Label1.Text = "Enter the path to the folder containing the documents to print"
$Label1.AutoSize = $true
$Label1.Location = New-Object System.Drawing.Point(50, 20)
$Label1.Font = New-Object System.Drawing.Font('Microsoft Sans Serif', 10)

$ButtonBrowse = New-Object System.Windows.Forms.Button
$ButtonBrowse.Text = "Browse"
$ButtonBrowse.Width = 80
$ButtonBrowse.Height = 30
$ButtonBrowse.Location = New-Object System.Drawing.Point(360, 48)
$ButtonBrowse.Font = New-Object System.Drawing.Font('Microsoft Sans Serif', 10)

# Function to handle the browse button click event
$ButtonBrowse.Add_Click({
    $folderBrowser = New-Object System.Windows.Forms.FolderBrowserDialog
    $result = $folderBrowser.ShowDialog()

    if ($result -eq [System.Windows.Forms.DialogResult]::OK) {
        $TextBox1.Text = $folderBrowser.SelectedPath
    }
})

$Button1 = New-Object System.Windows.Forms.Button
$Button1.Text = "Print"
$Button1.Width = 100
$Button1.Height = 30
$Button1.Location = New-Object System.Drawing.Point(150, 100)
$Button1.Font = New-Object System.Drawing.Font('Microsoft Sans Serif', 10)

# Function to handle the print button click event
$Button1.Add_Click({
    $folderPath = $TextBox1.Text
    Get-ChildItem -Path $folderPath -File | ForEach-Object {
        Write-Output "Printing file: $($_.FullName)"
        Start-Process -FilePath $_.FullName -Verb Print
    }
})

$ButtonExit = New-Object System.Windows.Forms.Button
$ButtonExit.Text = "Exit"
$ButtonExit.Width = 80
$ButtonExit.Height = 30
$ButtonExit.Location = New-Object System.Drawing.Point(360, 100)
$ButtonExit.Font = New-Object System.Drawing.Font('Microsoft Sans Serif', 10)

# Function to handle the exit button click event
$ButtonExit.Add_Click({
    $Form.Close()
})

$Form.Controls.AddRange(@($TextBox1, $Label1, $ButtonBrowse, $Button1, $ButtonExit))

[void]$Form.ShowDialog()


