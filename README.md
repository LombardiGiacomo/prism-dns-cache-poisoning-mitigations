# Analysis of the DNS Cache-Poisoning Attack using PRISM Model Checker

## 📌 Project Overview
This project presents a formal analysis of **DNS Cache-Poisoning Attacks** and the effectiveness of their mitigations. We utilized **PRISM** to model the behavior of the Domain Name System (DNS) and the race conditions inherent in these attacks using **Continuous Time Markov Chains (CTMC)**.

The study focuses on quantifying the probability of a successful cache poisoning attack under various network conditions and verifying the efficiency of the **DNS-CPM (DNS Cache Poisoning Mitigation)** architecture.

## 🎯 Scenarios Modeled
We analyzed two primary types of attacks and their respective defense mechanisms:

### 1. Kaminsky Attack (S-Type)
The attacker floods the resolver with forged responses, attempting to guess the correct combination of **Query ID** and **Port number** [5].
*   **Standard Mitigation (Randomization):** We modeled the standard defense where the resolver randomizes both the Query ID (16 bits) and the Source Port (~11 bits).
*   **DNS-CPM Mitigation (Rl1):** We modeled a detection module using the **Count-Min Sketch** algorithm to track suspicious responses and trigger mitigation.

### 2. Fragmentation Attack (SFrag-Type)
This attack exploits IP packet fragmentation. The attacker forces the resolver to request data requiring fragmentation and floods it with forged **2nd fragments**.
*   **Vulnerability:** The 2nd fragment lacks UDP/DNS headers, meaning the attacker only needs to guess the 16-bit **IPID**, and a larger attack window.
*   **DNS-CPM Mitigation (Rl2):** We modeled a detection rule that flags the arrival of a **1st fragment** as suspicious, forcing the session to switch to TCP via the TC bit.

## 🛠 Technologies
*   **PRISM Model Checker:** Used to build the formal models and verify properties.
*   **CTMC (Continuous Time Markov Chains):** Used to model real-time race conditions and delays.

## 📊 Key Results

### Kaminsky Attack Analysis
*   **Without DNS-CPM:** With high attacker guess rates and high server load, the probability of success remains significant even with randomization. Results are less critical if both Query ID and Port number are randomized.
*   **With DNS-CPM (Rl1):** The mitigation drastically reduces the success probability to approximately **0.0076%**, even if only Query ID randomization is active.

### Fragmentation (SFrag) Attack Analysis
*   **Without Mitigation:** The attack is highly effective, showing a success probability of up to **65%** due to the reduced entropy (only IPID needs guessing) and the wider attack window.
*   **With DNS-CPM (Rl2):** The model verified that the probability of success drops to **0.0%**. As soon as the legitimate 1st fragment is detected, the mitigation triggers a switch to TCP, preventing the cache from being poisoned by the forged 2nd fragments.

## 📂 Project Structure
*   `/Models`: Contains the PRISM code for S-Type and S-Frag attacks, both with and without DNS-CPM mitigation.
*   `/Properties`: Properties used to verify the models.
*   `/Graphs`: Graphs and property verification results.
*   `/Docs`: Reference papers and Presentation slides.

## 👥 Authors
*   Giacomo Lombardi
*   Nicolò Zarulli
