# Code Report #1: Impacts of Quantum Computing
In recent years, AI is taking over the world. Unless you've been residing under an igneous rock, you should be aware of both the optimism and scepticism in this new technology. However, there's a new technology that is silently making a breakthrough, and the impact of this tech may be even larger than the one AI made. You probably guessed it - Quantum Computing.

## What Impact?
For starters, Quantum Computing can retrieve data insanely quickly. The Grover's Search algorithm in Quantum Computing only has a time complexity of O(√N), which is much faster than the current best, O(N). This means that future database providers would need to completely revamp their databases, as it is unlikely that decade-old gold-standards like MySQL and Postgres would be the choice of the industry due to the obviously better choice available to them. This is a lot of work, and obviously involves the deprecation of some industry standards, but it means that the internet would speed up a considerable amount.

![Grovers vs Classical](https://miro.medium.com/v2/resize:fit:1086/0*yg_wETfn5a6nHkDG.png)
*Caption: Time complexity difference between Grover's Algorithm and a classical algorithm*


Moreover, as we've already seen with AI and NPUs, Quantum Computing would also lead to a rise of a completely new type of processing units: the QPU. This may not necessarily be a good thing, because it may mean processing units, like GPUs, can be partially ditched in some cases in favour for a better alternative. Unless NVIDIA starts becoming the industry standard in QPUs, they are going to lose a considerably large audience as well as a considerably large portion of their sales.

Likewise, Quantum Computing can go over massive amounts of passcode combinations instantly, and brute-force a password that would take years if not decades on a classical device, all in a frightening quick amount of time using Shor's algorithm. This would practically lead to the collapse of modern cybersecurity methods as we know it. The information we regard as "safe" today will be incredibly insecure in the future. However, we have ways to prevent this using Post-Quantum Cryptography. We have already developed some algorithms, such as CRYSTAL-Kyber and CRYSTAL-Dilithium, which were selected by the U.S. National Institute of Standards and Technology as quantum-safe encryption algorithms. However, updating cryptography standards to something as advanced as that is no easy task, and websites using legacy algorithms, or have trouble upgrading their platforms to become quantum-proof, will no longer be usable, and we would thus lose a large portion of the internet.

## The Current State of Quantum Computing
Currently, Quantum Computing is still an extremely experimental phase. We have developed many advanced pieces of technology in this field, such as Microsoft's Majorana 1, a topological QPU promising a staggering million qubits, that is revolutionising our current idea about the limits of Quantum Processing. Moreover, OpenQASM 3, an enhanced version of the OpenQASM 2, is currently rooting its features into many Quantum Computing simulators, including Qiskit by IBM and Cirq by Google.

![Majorana 1](https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcSCvX39Jo0DHRkncaH4HrDNPzQ4WZR0khg3Mg&s)
*Caption: Microsoft's Majorana 1 QPU chip*


## What am I doing?
To keep up with this revolution, I am currently developing [QUCOM](https://github.com/frogtheastronaut/qucom), a blazingly fast Quantum Computing simulation SDK for the web. It uses Rust, which is then compiled to WASM, which can then be imported as a CDN or as a npm package into your web project. I am currently implementing OpenQASM 3 support, as well as the ability to parse QASM and execute it directly in the browser. Please stay tuned for more information!

## What can you do?
If you want to be involved in the Quantum Revolution, here are some ways to take part:
1. Contribute to open-source projects, such as IBM's Qiskit or my own project (I always accept pull requests!)
2. Learn Quantum Computing. To get started, learn matrix multiplication, Bra-ket notation, and superposition and entanglement. It also doesn't hurt to know some logic gates, Hadamard, as well as some simple algorithms. This all takes from a few weeks to maybe a few months. I may release a Youtube playlist that will help you get started!

## Conclusion
In the ever-changing world, AI probably won't last as long as everyone thinks, and one day Quantum Computing is bound to make a big impact on our world. Thank you for reading, and see you in the next one!