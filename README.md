Download Link: https://assignmentchef.com/product/solved-cloud-computing-homework-3
<br>
A hypercall is a way for a guest OS to make a call to the hypervisor, in some ways similar to how a system call allows an application to make a call to the OS. We are asking you to write a hypercall to become familiar with how they work and the codebase for KVM. For this part of the assignment, you will also set up your VM so you can use it for your hypercall development.

The prototype for your hypercall will be the following. The argument vcpu_id contains the CPU id in your VM.The hypercall you write should take one argument and output the information about virtual CPU in KVM. Modern architectures provide special support for virtualization and add a privileged instruction for hypercalls, which you will use in this part of the assignment. Rather than using the default kernel that comes with Ubuntu 14.04.5, you should download and build a more recent version of the Linux kernel, kernel version 4.10, and install that in your VM. It will be this updated version of the kernel/KVM source that you will modify to implement your hypercall.

The followings are other features you need to implement in your hypercall. Your code should handle errors properly. At a minimum, your hypercall should detect and report the condition which the virtual CPU id passed by the VM is not valid and return error code to the VM. You should assign an unique number for your hypercall, so KVM can identify the call and handle it properly.

HINT: Intel’s (most likely CPU you have in your personal computer) virtualization extension (VMX) provides a “privileged” instruction called vmcall for hypercall. When the VM executes the instruction, it traps from non-root operation to root operation so the hypervisor can then handle the hypercall. You should search the Linux source and look at how vmcall is handled by KVM.

HINT: You can use the Linux Cross-Reference (LXR) to investigate different hypercalls already defined. You should use the same calling convention as the other hypercalls.

HINT: You should look at how the structure kvm_vcpu is defined and used in the source code.

<strong>Compiling / Updating Linux/KVM host:</strong>

To enable ftrace in your KVM host, go to <strong>Kernel Hacking</strong> and select <strong>Tracers</strong>. Then enter <strong>Tracers</strong>, select both <strong>Kernel Function Tracer</strong> and <strong>Kernel Function Graph Tracer</strong>. Finally, save the config and exit.

After you add your hypercall to your Linux/KVM source, use the following command to compile the kernel.

<h2>Q2: Test your new hypercall</h2>

To test your new hypercall, you will install your modifications in the guest kernel running in your nested VM for testing. As mentioned earlier, the instruction that initiates a hypercall is a privileged instruction and cannot be executed in user mode. To test the hypercall from user space, you need to add a new system call in your guest kernel which in turn calls to the vcpu_info. The prototype of the system call can be defined as the following.

Your sys<em>vcpu</em>info system call should enumerate each of the online CPU, and pass the CPU id to KVM via the vcpu_info hypercall.

You should write a simple C program which calls to sys<em>vcpu</em>info from user space. You can get the see the output from the hypercall in your KVM host using the following command:

NOTE: Although system calls are generally accessed through a library (libc), your test program should access your system call directly. This is accomplished by utilizing the general purpose syscall(2) system call (consult its man page for more details).