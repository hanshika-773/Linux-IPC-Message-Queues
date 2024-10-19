# Linux-IPC-Message-Queues
Linux IPC-Message Queues

# AIM:
To write a C program that receives a message from message queue and display them

# DESIGN STEPS:

### Step 1:

Navigate to any Linux environment installed on the system or installed inside a virtual environment like virtual box/vmware or online linux JSLinux (https://bellard.org/jslinux/vm.html?url=alpine-x86.cfg&mem=192) or docker.

### Step 2:

Write the C Program using Linux message queues API 

### Step 3:

Execute the C Program for the desired output. 

# PROGRAM:

## C program that receives a message from message queue and display them
```
// C Program for Message Queue (Writer Process) 
#include <stdio.h> 
#include <sys/ipc.h> 
#include <sys/msg.h> 

// structure for message queue 
struct mesg_buffer 
{ 
	long mesg_type; 
	char mesg_text[100]; 
}message; 
int main() 
{ 	
        key_t key; 
	int msgid; 
        // ftok to generate unique key 
	key = ftok("progfile", 65); 
	// msgget creates a message queue 
	// and returns identifier 
	msgid = msgget(key, 0666 | IPC_CREAT); 
        message.mesg_type = 1; 
	printf("Write Data : "); 
	gets(message.mesg_text); 
	// msgsnd to send message 
	msgsnd(msgid, &message, sizeof(message), 0); 
	// display the message 
	printf("Data sent is : %s \n", message.mesg_text); 
	return 0; 
}


// C Program for Message Queue (Reader Process)
#include <stdio.h>
#include <sys/ipc.h>
#include <sys/msg.h>

// structure for message queue
struct mesg_buffer 
{
	long mesg_type;
	char mesg_text[100];
}message;
int main()
{
	key_t key;
	int msgid;
        // ftok to generate unique key
	key = ftok("progfile", 65);
	// msgget creates a message queue
	// and returns identifier
	msgid = msgget(key, 0666 | IPC_CREAT);
	// msgrcv to receive message
	msgrcv(msgid, &message, sizeof(message), 1, 0);
	// display the message
        printf("Data Received is : %s \n",message.mesg_text);
        // to destroy the message queue
	msgctl(msgid, IPC_RMID, NULL);
	return 0;
}
```

# OUTPUT
## Writer Process
![378045800-6dc2acb7-5fd3-4ae9-80e3-2144690075cf](https://github.com/user-attachments/assets/ef03afd2-ae7d-4b8b-aced-4749f9b439f1)
![378045805-5322ed78-202d-45f8-ac3e-d763a1f3e786](https://github.com/user-attachments/assets/b3152372-077b-4727-9ec7-537ac6b2c991)

## Reader Process
![378045814-4861fd22-497b-4d2a-b85e-a48ac8841e79](https://github.com/user-attachments/assets/c19c1df2-0ee7-47c1-acf6-d913ba87be47)


# RESULT:
The programs are executed successfully.
