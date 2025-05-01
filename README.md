"Do the right thing by default, but make it introspectable and fixable."
                                                             ~ Rudy
## Note: This manifesto is being written to share the ideas with others , changes are allowed but copying the ideas isn't since i have trademarked these ideas by writing the manifesto.

# Why did i write this manifesto? (CH001)
The main cause of writing this manifesto is because of Unix's limitations and the limitations i've faced on Linux , It's generally UNIX as a whole
What are these limitations? 
Well lets first discuss about the Linux limitations:
- It is too entrenched in legacy design decisions (e.g. init systems, filesystem layout, permissions).
- It suffers from codebase complexity, excessive abstractions, and conflicting philosophies.
- It has weak isolation, overexposed root privileges, and patchwork security models.
- Package management is messy, and configuration is inconsistent across distros.

While BSD (Berkeley Software Distribution) tried to solve this , its made for big routers and servers mostly and lacks destkop support and rapid development of latest technology because of the Lawsuits faced by UNIX Laboratory , which is why big tech giants don't produce drivers for BSD and that's why Linux is the main attraction.

Now about Unix generally as a whole:
- The 1970s file system hierarchy doesn’t scale with modern application design.
- The process and permission model was designed before containers, cloud, or distributed computing.
- The interface between hardware and software is inconsistent and wasteful.

And how will i solve these problems is well first of all the Unix problem , since i am not gonna make any other linux distro , since there are too many out there , RadianOS will be an independent group of OSes similar to Unix , here are my solutions to solving the unix problems:

Problem 1. Filesystem Hierarchy - Its old and not scalable with modern applications and my Vision for it is as follows:
- Namespace-based FS: Apps are isolated into their own directory (/apps/editor, /apps/browser) with everything self-contained.
- Declarative config storage: Use /system, /config, /users rather than scattering files across /etc, /var, etc.
- Immutable base system with overlays for changes (like NixOS or Fedora Silverblue).

Problem 2. Everything is a File (Even When It Shouldn't Be)
Unix's "everything is a file" model forces clunky APIs (e.g. IOCTLs, file descriptors for sockets, weird abstraction leaks).
My solution:
- Create a cleaner object model:
  - Separate event streams, memory-mapped regions, and data stores from each other.
  - Use a clear capability-based IPC model (similar to microkernels).

- Use typed system handles, not just file descriptors.

Problem 3. Root Privileges Are Too Powerful
The root user can do everything; sandboxing is bolted on (via seccomp, namespaces, etc).
My Solution:
- No root user. Ever.
- Use capability-based access control: Every process starts with a limited set of capabilities, like object-capability systems (e.g. seL4, CapROS).
- Make privilege escalation explicit, requestable, and revocable (think: OAuth, but for system rights).

Problem 4. Init System Complexity
Init systems like systemd or SysVinit are either messy, overly complex, or too rigid. Enterprises have ruined Systemd and its broken by design https://ewontfix.com/14/
My Solution:
- Build a single, event-driven service manager that’s:
  - Observable (logs are structured, inspectable in real time)
  - Reactive (on-demand service loading, dependency tracking)
  - Scriptable (with a small DSL or config language, like TOML/YAML)

Problem 5. Package Management Mess
DEB, RPM, APT, YUM, Pacman, flatpaks, AppImages, etc. create a fractured and inconsistent ecosystem.
My Solution:
- Design a content-addressed, signed, sandboxed package system:

  - Inspired by Nix (but simpler) - The system will not be centered around the language (DSL).
  - Fully reproducible builds
  - Declarative system state (e.g. "this system should have apps A, B, C at version X" and also permissions.)
  - These Packages would be accepted after a thorrow inspection for any malicious codes that also adds up one more point
  - No executable files - While they might seem like an easy way to install softwares , they run on system level potentially allowing malicious code to execute ( EXE files on Windows explained ).

Problem 6. Tooling Assumes Too Much Knowledge
CLIs and configuration tools are user-hostile. Logs are cryptic. Errors are unhelpful.
My Solution:
- Every system tool should:

  - Explain itself on failure (like cargo, rustc, or git do now)
  - Offer interactive help and guided fixes
  -  Use consistent, well-designed CLI interfaces
