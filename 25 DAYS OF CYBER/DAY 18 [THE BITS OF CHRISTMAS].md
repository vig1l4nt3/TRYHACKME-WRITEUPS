DAY 18 - THE BITS OF CHRISTMAS [REVERSE ENGINEERING]
----

# STORY :
----

>>"Silly Santa...Forgetting his password yet again!" complains Elf McEager. However, it is in fact Elf McEager who is silly for not creating a way to reset Santa's password for the TBFC dashboard.

>>Santa needs to get back into the dashboard for Christmas! Can you help Elf McEager reverse engineer TBFC's application to retrieve the password for Santa?!

----

# GETTING STARTED : 
----

>>You got your hands dirty with everything that is radare2 yesterday. Today, however, we're going to be taking a look at a more interactive approach of disassembling an application.

>>Due to its compatibility and long history, the .NET Framework is a popular platform for software developers to develop software with. Anything Windows or web, .NET will cover it.

>>For example, I developed my answer to Microsoft's Calculator in .NET:

>>This is quite a trivial use of .NET, but hey, it works (trust me on this one okay?). Whilst you may not want to take a look behind the code of this application, there are some that may be of interest such as in the challenge today. Let's take a look at the application below:

>>When running the application, we are asked for an input (in this case a Username). This begs the question, how does the application know what username/password is right or wrong? The application must know the answer...Applications that are created using the  .NET framework can be disassembled using tools such as ILSpy or Dotpeek.

>>Loading our calculator application into ILSpy verifies that it is indeed a .NET application:

>>After expanding some of the resources, we can see references to elements of the application such as buttons, labels and the likes:

>>When looking through the objects, ILspy has helpfully been able to recreate what some of the source code behind the application is:

>>Because it's a calculator, we can see the c++ code that checks for mathematical operators (plus, minus, multiply and divide). Looking through other objects reveals similar code (of that we'd expect of a Calculator at least).

----

# Challenge:
----

>>Deploy the instance attached to this task and log in using the Remote Desktop Protocol (RDP). Open the application "TBFC_APP.exe" on the Desktop and enter the correct password!
You can use "Remmina" on the TryHackMe AttackBox to connect to the instance with the following credentials, or any RDP client such as Microsofts if you wish to connect to the TryHackMe VPN:

IP Address: MACHINE_IP
Username: cmnatic
Password: Adventofcyber! 

1.Navigate to the "Applications" tab on the AttackBox where "Remmina" is located in the "Internet" sub-menu. 

2.Reminna will ask you for a password to save sessions, we can safely press "Cancel":

3.Now fill out the IP address of the target Instance that you have deployed, input the Username and password provided and set your "Color depth" to "RemoteFX (32 bpp) like so: 

----

# MY DOCUMENTATION :
----

>>AFTER LOGIN TO RDP DESKTOP USING REMINA AND ADDED THE TBFC APP INTO THE ILSPY .
>>THAT APP DISLAYS ABOU THE FRAMEWORK IT USES.WHICH IS .NET

```js
// C:\Users\cmnatic\Desktop\TBFC_APP.exe
// CrackMe, Version=0.0.0.0, Culture=neutral, PublicKeyToken=null
// Global type: <Module>
// Entry point: <Module>.main
// Architecture: x86
// This assembly contains unmanaged code.
// Runtime: v4.0.30319
// Hash algorithm: SHA1

using System.Reflection;
using System.Runtime.Versioning;
using System.Security;
using System.Security.Permissions;

[assembly: SecurityRules(SecurityRuleSet.Level1)]
[assembly: TargetFramework(".NETFramework,Version=v4.6.1", FrameworkDisplayName = ".NET Framework 4.6.1")]
[assembly: SecurityPermission(SecurityAction.RequestMinimum, SkipVerification = true)]
[assembly: AssemblyVersion("0.0.0.0")]

```

>>AFTER ANALYSING SOMEFILE THERE IS A CRACKME FILE.NOW EXAMINE THE CRACKME FILE IT HAS TWO SUB FOLDER "MAIN_FORM" AND "DATA_FORM".
>>THIS IS THE MAIN_FORM FILE   
>>WHICH HAS THE CODE OF ALL THE SUB-FILE CREATED IN THIS MAIN_FORM.
>>IF WE ANALYSE THE CODE IN THIS FILE WE GET OUR FLAG AND THE PASSWORD FOR THE TBFC APP. 

```js [main_form]
// CrackMe.MainForm
using System;
using System.ComponentModel;
using System.Drawing;
using System.Runtime.CompilerServices;
using System.Runtime.ExceptionServices;
using System.Runtime.InteropServices;
using System.Windows.Forms;
using CrackMe;

public class MainForm : Form
{
	private Label labelKey;

	private TextBox textBoxKey;

	private Panel panelLogo;

	private TableLayoutPanel tableLayoutPanel1;

	private Button buttonActivate;

	private TableLayoutPanel tableLayoutPanelButtons;

	private Label labelOrg;

	private Container components;

	public MainForm()
	{
		try
		{
			InitializeComponent();
		}
		catch
		{
			//try-fault
			base.Dispose(disposing: true);
			throw;
		}
	}

	private void ~MainForm()
	{
		((IDisposable)components)?.Dispose();
	}

	private void InitializeComponent()
	{
		System.ComponentModel.ComponentResourceManager resources = new System.ComponentModel.ComponentResourceManager(typeof(CrackMe.MainForm));
		labelKey = new System.Windows.Forms.Label();
		textBoxKey = new System.Windows.Forms.TextBox();
		panelLogo = new System.Windows.Forms.Panel();
		tableLayoutPanel1 = new System.Windows.Forms.TableLayoutPanel();
		buttonActivate = new System.Windows.Forms.Button();
		tableLayoutPanelButtons = new System.Windows.Forms.TableLayoutPanel();
		labelOrg = new System.Windows.Forms.Label();
		tableLayoutPanel1.SuspendLayout();
		tableLayoutPanelButtons.SuspendLayout();
		SuspendLayout();
		labelKey.Anchor = System.Windows.Forms.AnchorStyles.Right;
		labelKey.AutoSize = true;
		System.Drawing.Point location = new System.Drawing.Point(30, 14);
		labelKey.Location = location;
		labelKey.Name = "labelKey";
		System.Drawing.Size size = new System.Drawing.Size(56, 13);
		labelKey.Size = size;
		labelKey.TabIndex = 0;
		labelKey.Text = "Password:";
		labelKey.Click += new System.EventHandler(labelKey_Click);
		textBoxKey.Anchor = System.Windows.Forms.AnchorStyles.Left;
		System.Drawing.Point location2 = new System.Drawing.Point(92, 10);
		textBoxKey.Location = location2;
		textBoxKey.Name = "textBoxKey";
		System.Drawing.Size size2 = new System.Drawing.Size(160, 20);
		textBoxKey.Size = size2;
		textBoxKey.TabIndex = 1;
		textBoxKey.TextChanged += new System.EventHandler(textBoxKey_TextChanged);
		panelLogo.BackgroundImage = (System.Drawing.Image)resources.GetObject("panelLogo.BackgroundImage");
		panelLogo.BackgroundImageLayout = System.Windows.Forms.ImageLayout.Center;
		panelLogo.Dock = System.Windows.Forms.DockStyle.Top;
		System.Drawing.Point location3 = new System.Drawing.Point(0, 0);
		panelLogo.Location = location3;
		panelLogo.Name = "panelLogo";
		System.Drawing.Size size3 = new System.Drawing.Size(299, 100);
		panelLogo.Size = size3;
		panelLogo.TabIndex = 2;
		panelLogo.Paint += new System.Windows.Forms.PaintEventHandler(panelLogo_Paint);
		tableLayoutPanel1.ColumnCount = 2;
		tableLayoutPanel1.ColumnStyles.Add(new System.Windows.Forms.ColumnStyle(System.Windows.Forms.SizeType.Percent, 30f));
		tableLayoutPanel1.ColumnStyles.Add(new System.Windows.Forms.ColumnStyle(System.Windows.Forms.SizeType.Percent, 70f));
		tableLayoutPanel1.Controls.Add(labelKey, 0, 0);
		tableLayoutPanel1.Controls.Add(textBoxKey, 1, 0);
		tableLayoutPanel1.Dock = System.Windows.Forms.DockStyle.Bottom;
		System.Drawing.Point location4 = new System.Drawing.Point(0, 137);
		tableLayoutPanel1.Location = location4;
		tableLayoutPanel1.Name = "tableLayoutPanel1";
		tableLayoutPanel1.RowCount = 1;
		tableLayoutPanel1.RowStyles.Add(new System.Windows.Forms.RowStyle(System.Windows.Forms.SizeType.Percent, 100f));
		System.Drawing.Size size4 = new System.Drawing.Size(299, 41);
		tableLayoutPanel1.Size = size4;
		tableLayoutPanel1.TabIndex = 8;
		buttonActivate.Anchor = System.Windows.Forms.AnchorStyles.None;
		System.Drawing.Point location5 = new System.Drawing.Point(111, 11);
		buttonActivate.Location = location5;
		buttonActivate.Name = "buttonActivate";
		System.Drawing.Size size5 = new System.Drawing.Size(75, 23);
		buttonActivate.Size = size5;
		buttonActivate.TabIndex = 3;
		buttonActivate.Text = "Submit Password";
		buttonActivate.UseVisualStyleBackColor = true;
		buttonActivate.Click += new System.EventHandler(buttonActivate_Click);
		tableLayoutPanelButtons.ColumnCount = 3;
		tableLayoutPanelButtons.ColumnStyles.Add(new System.Windows.Forms.ColumnStyle(System.Windows.Forms.SizeType.Percent, 33.33333f));
		tableLayoutPanelButtons.ColumnStyles.Add(new System.Windows.Forms.ColumnStyle(System.Windows.Forms.SizeType.Percent, 33.33334f));
		tableLayoutPanelButtons.ColumnStyles.Add(new System.Windows.Forms.ColumnStyle(System.Windows.Forms.SizeType.Percent, 33.33334f));
		tableLayoutPanelButtons.Controls.Add(labelOrg, 0, 0);
		tableLayoutPanelButtons.Controls.Add(buttonActivate, 1, 0);
		tableLayoutPanelButtons.Dock = System.Windows.Forms.DockStyle.Bottom;
		System.Drawing.Point location6 = new System.Drawing.Point(0, 178);
		tableLayoutPanelButtons.Location = location6;
		tableLayoutPanelButtons.Name = "tableLayoutPanelButtons";
		tableLayoutPanelButtons.RowCount = 1;
		tableLayoutPanelButtons.RowStyles.Add(new System.Windows.Forms.RowStyle(System.Windows.Forms.SizeType.Percent, 33.33333f));
		System.Drawing.Size size6 = new System.Drawing.Size(299, 46);
		tableLayoutPanelButtons.Size = size6;
		tableLayoutPanelButtons.TabIndex = 7;
		labelOrg.Dock = System.Windows.Forms.DockStyle.Fill;
		System.Drawing.Color grayText = System.Drawing.SystemColors.GrayText;
		labelOrg.ForeColor = grayText;
		System.Drawing.Point location7 = new System.Drawing.Point(3, 0);
		labelOrg.Location = location7;
		labelOrg.Name = "labelOrg";
		System.Drawing.Size size7 = new System.Drawing.Size(93, 46);
		labelOrg.Size = size7;
		labelOrg.TabIndex = 4;
		labelOrg.Text = "The Best Festival Company 2020";
		labelOrg.TextAlign = System.Drawing.ContentAlignment.MiddleCenter;
		base.AcceptButton = buttonActivate;
		System.Drawing.SizeF sizeF2 = (base.AutoScaleDimensions = new System.Drawing.SizeF(6f, 13f));
		base.AutoScaleMode = System.Windows.Forms.AutoScaleMode.Font;
		System.Drawing.Size size9 = (base.ClientSize = new System.Drawing.Size(299, 224));
		base.Controls.Add(tableLayoutPanel1);
		base.Controls.Add(tableLayoutPanelButtons);
		base.Controls.Add(panelLogo);
		base.FormBorderStyle = System.Windows.Forms.FormBorderStyle.FixedSingle;
		base.Icon = (System.Drawing.Icon)resources.GetObject("$this.Icon");
		base.MaximizeBox = false;
		base.MinimizeBox = false;
		base.Name = "MainForm";
		Text = "TBFC Dashboard";
		base.Load += new System.EventHandler(MainForm_Load);
		tableLayoutPanel1.ResumeLayout(false);
		tableLayoutPanel1.PerformLayout();
		tableLayoutPanelButtons.ResumeLayout(false);
		ResumeLayout(false);
	}

	private void MainForm_Load(object sender, EventArgs e)
	{
	}

	private void buttonExit_Click(object sender, EventArgs e)
	{
		Close();
	}

	private void buttonAbout_Click(object sender, EventArgs e)
	{
		new AboutForm().ShowDialog();
	}

	private unsafe void buttonActivate_Click(object sender, EventArgs e)
	{
		IntPtr value = Marshal.StringToHGlobalAnsi(textBoxKey.Text);
		sbyte* ptr = (sbyte*)System.Runtime.CompilerServices.Unsafe.AsPointer(ref <Module>.??_C@_0BB@IKKDFEPG@santapassword321@);
		void* ptr2 = (void*)value;
		byte b = *(byte*)ptr2;
		byte b2 = 115;
		if ((uint)b >= 115u)
		{
			while ((uint)b <= (uint)b2)
			{
				if (b != 0)
				{
					ptr2 = (byte*)ptr2 + 1;
					ptr++;
					b = *(byte*)ptr2;
					b2 = (byte)(*ptr);
					if ((uint)b < (uint)b2)
					{
						break;
					}
					continue;
				}
				MessageBox.Show("Welcome, Santa, here's your flag thm{046af}", "That's the right key!", MessageBoxButtons.OK, MessageBoxIcon.Asterisk);
				return;
			}
		}
		MessageBox.Show("Uh Oh! That's the wrong key", "You're not Santa!", MessageBoxButtons.OK, MessageBoxIcon.Hand);
	}

	private void panelLogo_Paint(object sender, PaintEventArgs e)
	{
	}

	private void textBoxKey_TextChanged(object sender, EventArgs e)
	{
	}

	private void labelKey_Click(object sender, EventArgs e)
	{
	}

	[HandleProcessCorruptedStateExceptions]
	protected override void Dispose([MarshalAs(UnmanagedType.U1)] bool A_0)
	{
		if (A_0)
		{
			try
			{
				~MainForm();
			}
			finally
			{
				base.Dispose(disposing: true);
			}
		}
		else
		{
			base.Dispose(disposing: false);
		}
	}
}
```

----

# ANSWER THE FOLLOWING :
----

Open the "TBFC_APP" application in ILspy and begin decompiling the code
>>No answer needed

What is Santa's password?
>>santapassword321

Now that you've retrieved this password, try to login...What is the flag?
>>thm{046af}

----