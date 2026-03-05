<div align="center">

```
  ███████╗██████╗ ███████╗ ██████╗████████╗██████╗ ███████╗
  ██╔════╝██╔══██╗██╔════╝██╔════╝╚══██╔══╝██╔══██╗██╔════╝
  ███████╗██████╔╝█████╗  ██║        ██║   ██████╔╝█████╗
  ╚════██║██╔═══╝ ██╔══╝  ██║        ██║   ██╔══██╗██╔══╝
  ███████║██║     ███████╗╚██████╗   ██║   ██║  ██║███████╗
  ╚══════╝╚═╝     ╚══════╝ ╚═════╝   ╚═╝   ╚═╝  ╚═╝╚══════╝
                           .-++++++-                            
                    .+###+-.        .-+###-                     
                .###.                      -##-                 
              ##+                              ##-              
           +#+                                    ##            
         -#+                                        ##          
        ##               .###########+               .#+        
      -#.              .################               ##       
     +#                #################-               .#      
    -#                 #################-                .#     
   -#      .-+++++.    #################.    .-+++-.      .#    
   #    .####  .#####   ################   ####+   ####    +#   
  +-    ###       ###+  +#..-######..##.  +##-       ##-    #.  
  #    +##.       ####  +#   .###+   -#. .###        ###    .#  
 +#    +###       .#### ####### .######. ####       -###     #. 
 #-     ########-  +###  --+########--. .###   ########.     #+ 
 #         .        .###### #######+ #####-                  ## 
 #      .#############+ .-- .--.-.- .-.  #############-      ## 
 #.     #######+-############## .####+########--#######-     #+ 
 #-    .####+        #######- + #. ######+         ####+     #- 
 +#     ###                 -##.##+-                -##.     #  
  #     +##        .#   #######.-######-  ++        .##     -+  
  ++     ##.       ## #####+.     --+####--#.       .#+     #   
   #-    -##     +##.###               +##.###      ##     #+   
    #     -########-+#+      .#.+#       ## #########     +#    
    .#.     #######-##        +##        ###.######.     +#     
      #.      ++-. ###-       .##        ###- .-++      ##      
       ##          ####-.     ###-     .####.         .#-       
        -#-          -#########+##########           ##         
          +#.          -#######  #######           ##.          
            -#+           +-        -.          .##             
               ##-                            ##-               
                  +##-                   .###-                  
                      -+####++----++####+.                      
                                                                
```
*An original Insane-rated machine for [Hack The Box](https://hackthebox.com)*

---

*"The observer changes what is observed."*

---

</div>

## Status

This machine is currently to be uploaded for a review by Hack The Box.

This page is a placeholder. All contents are filtered to ensure nothing is leaked. Full source code, build tooling, developer notes, and a detailed retrospective will be published here after the machine is officially retired. Until then, nothing about the internal design, implementation, or intended solution path is disclosed.

---

## What This Is

SPECTRE is a machine I designed, implemented, and automated from scratch over the course of time. It is rated **Insane**.

The premise is a hardened internal analytics platform — a realistic multi-service environment where every vulnerability exists because of a documented architectural decision, not because a service needed to be broken for the sake of it. The lore is internally consistent. Developers argue in notes. Tickets are filed and backlogged. The timeline makes sense. If you sat down to use this system as a normal operator, not knowing it was meant to be hacked, it would behave like a real one.

The exploitation chain is multiple steps. None of them can be skipped. No step is a guess — every one follows directly from something already discovered. The final privilege escalation involves a custom component I wrote specifically for this machine. Understanding how it works — which is discoverable — is the only path through.

The build is fully automated: a single script, * stages, idempotent, with a security audit and remediation pass built in. Total provisioning time is approximately 17 minutes. This is by design due to Hack The Box VM resource limitations. 

---

## Design Philosophy

The most interesting security failures in the real world are not single vulnerabilities. They are chains — each link individually defensible, some even documented as known risk, but together constituting something critical.

Building SPECTRE was an exercise in modelling that accurately. The question driving every design decision was: *would a real organisation make this choice?* If the answer was no — if the only reason for something to exist was to be exploited — it was redesigned until the answer was yes.

This constraint is harder than it sounds. It means the attack surface has to emerge naturally from the architecture rather than being bolted on. It means the idea, scenario and lore has to be written first, not retrofitted. It means the difficulty calibration has to be done honestly — not by making things obscure, but by making them genuinely complex.

HTB's own guidance on machine design describes the problem well: "simply slapping a few vulnerable applications together with no real narrative or reason for their existence comes across as CTF-y and can just be an overall bad experience for the player." SPECTRE was built against that standard.

---

## Reading

The following is a record of what informed how I think about adversarial system design — not a bibliography of the implementation.

**On how systems fail**

- Anderson, R. (2020). *Security Engineering: A Guide to Building Dependable Distributed Systems*, 3rd ed. Wiley. The most complete treatment of how security properties are lost across the full stack — from cryptographic primitives to organisational processes. Chapter 3 on psychology and usability is as relevant as any technical chapter.

- Saltzer, J. H. & Schroeder, M. D. (1975). *The Protection of Information in Computer Systems.* Proceedings of the IEEE, 63(9). The eight design principles articulated here — economy of mechanism, fail-safe defaults, complete mediation, least privilege — remain the clearest framework for reasoning about where security controls break down.

- Schneier, B. (1999). *Attack Trees.* Dr. Dobb's Journal. Short, but the mental model for decomposing multi-stage compromise into tree structures is foundational for designing chains rather than isolated findings.

**On adversarial thinking**

- Mitre ATT&CK Framework. *Enterprise Tactics, Techniques and Procedures.* The reference for asking: does this chain resemble something that happens in practice? Each step in SPECTRE maps to real documented adversary behaviour — by design.

- Hutchins, E., Cloppert, M., Amin, R. (2011). *Intelligence-Driven Computer Network Defense Informed by Analysis of Adversary Campaigns and Intrusion Kill Chains.* Lockheed Martin. The kill chain model is useful not just for defenders — understanding how an attacker must progress stage by stage is exactly the right framing for designing a machine where no step can be skipped.

**On CTF design as a discipline**

- Vigna, G. et al. (2014). *Ten Years of iCTF: The Good, The Bad, and The Ugly.* USENIX ASE. On what separates challenges that teach real skills from challenges that are purely puzzles. The distinction matters.

- Bock, K. et al. (2021). *Your Censor is My Proxy: Circumventing Censorship with the N-CDN Protocol.* USENIX Security. Not directly about CTF design — but a useful case study in how a complex multi-layer system creates unintended trust relationships at the intersections. This is the kind of thinking SPECTRE was built around.

---

## After Retirement

When HTB retires this machine, this repository will contain everything: the full build script, all source code, the intended solution path, design decisions, what broke during development, what I would do differently and what I've learned.

---

## Author

Built by **xmp** — 2024/2026

---

<div align="center">

```
  ╔══════════════════════════════════════════════════════════════╗
  ║           Unauthorized access is monitored and               ║
  ║         prosecuted. All sessions are logged and              ║
  ║    audited. You have been warned. Proceed accordingly.       ║
  ╚══════════════════════════════════════════════════════════════╝
```

</div>
