## Week 6 Homework Submission File: Advanced Bash - Owning the System

Please edit this file by adding the solution commands on the line below the prompt. 

Save and submit the completed file for your homework submission.

**Step 1: Shadow People** 

1. Create a secret user named `sysd`. Make sure this user doesn't have a home folder created:
    - sudo adduser sysd --no-create-home
2. Give your secret user a password: 
    - sysd
    - very easy to remember...
3. Give your secret user a system UID < 1000:
    - usermod -u 47 sysd
4. Give your secret user the same GID:
   - groupmod -g 47 sysd
5. Give your secret user full `sudo` access without the need for a password:
   - 
   
        -visudo    
        -Add at the bottom:
        -sysd all=(all) NOPASSWD:ALL
![Edit_Sudoers_Step1_Q5](/HW-6/Edit_Sudoers_Step1_Q5.png)
6. Test that `sudo` access works without your password:
![Sudo-L_Step1_Q6](/HW-6/Sudo-L_Step1_Q6.png)

        sudo cat /etc/shadow
        sudo visudo
        None of them required a password.

**Step 2: Smooth Sailing**

1. Edit the `sshd_config` file:

    ```bash
    
    ```

**Step 3: Testing Your Configuration Update**
1. Restart the SSH service:
    - `Your solution command here`

2. Exit the `root` account:
    - `Your solution command here`

3. SSH to the target machine using your `sysd` account and port `2222`:
    - `Your solution command here`

4. Use `sudo` to switch to the root user:
    - `Your solution command here`

**Step 4: Crack All the Passwords**

1. SSH back to the system using your `sysd` account and port `2222`:

    - `Your solution command here`

2. Escalate your privileges to the `root` user. Use John to crack the entire `/etc/shadow` file:

    - `Your solution command here`

---

© 2020 Trilogy Education Services, a 2U, Inc. brand. All Rights Reserved.

