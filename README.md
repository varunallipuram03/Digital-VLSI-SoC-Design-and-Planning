# 🚀 Digital VLSI SoC Design and Planning

## VSD OpenLANE SKY130 Workshop Documentation

This repository documents my complete learning journey through the **VSD OpenLANE SKY130 Workshop**.

The workshop focuses on understanding the complete ASIC RTL-to-GDSII flow using OpenLANE, OpenROAD, SKY130 PDK and other open-source EDA tools.

---

## 👨‍💻 Author
Author: Varun Venkata Allipuram

contact: +91 9177396816

Email: varunallipuram@gmail.com

Platform: GitHub Codespaces

PDK: SKY130A

OpenLane Version: v1.0.2

Workshop: VSD OpenLane Sky130 Workshop

---

## 📚 Workshop Progress

| Day   | Topic                                                               | Status |
| ----- | ------------------------------------------------------------------- | ------ |
| Day 1 | Inception of Open-Source EDA, OpenLANE and SKY130 PDK               | ✅      |
| Day 2 | Good Floorplan vs Bad Floorplan and Introduction to Library Cells   | ✅      |
| Day 3 | Design Library Cell using Magic Layout and ngspice Characterization | ✅      |
| Day 4 | Pre-layout Timing Analysis and Importance of Good Clock Tree        | ✅      |
| Day 5 | Final Steps for RTL2GDS using TritonRoute and OpenSTA               | ✅      |

---

## 🛠 Tools Explored

* OpenLANE
* OpenROAD
* Yosys
* ABC
* OpenSTA
* Magic
* Netgen
* TritonRoute
* SKY130 PDK

---

## 🎯 Learning Objectives

* Understand ASIC Design Flow
* Explore Open-Source Physical Design
* Learn RTL-to-GDSII Flow
* Study Floorplanning and Placement
* Analyze Timing and Clock Tree Synthesis
* Understand Routing and Signoff

---

## 📁 Documentation

### Day 1

📄 Open-Source EDA, OpenLANE and SKY130 PDK

### Day 2

📄 Floorplanning and Library Cells

### Day 3

📄 Magic Layout and ngspice Characterization

### Day 4

📄 Timing Analysis and Clock Tree Synthesis

### Day 5

📄 RTL2GDSII using TritonRoute and OpenSTA

---

## ⭐ Repository Highlights

✔ Detailed Notes

✔ Practical Lab Commands

✔ Screenshots and Results

✔ Engineering Insights

✔ Interview Questions

✔ Daily Learnings

---

### Workshop Platform

VSD OpenLANE SKY130 Workshop

# RTL-to-GDSII Physical Design Flow using OpenLANE and SKY130

## Overview

This repository documents my hands-on journey through the complete RTL-to-GDSII ASIC Physical Design flow using the OpenLANE framework and the SKY130 Process Design Kit (PDK). Throughout this workshop, I explored the fundamental stages involved in transforming a Register Transfer Level (RTL) design into a fabrication-ready integrated circuit layout.

The flow covered every major step of ASIC implementation, beginning with RTL synthesis and progressing through floorplanning, placement, clock tree synthesis, routing, physical verification, and parasitic extraction. In addition to understanding the theoretical concepts behind each stage, I performed practical experiments using industry-relevant open-source EDA tools and analyzed their impact on timing, area, power, and manufacturability.

The primary objective of this workshop was to gain a comprehensive understanding of modern digital ASIC design methodologies while developing hands-on experience with physical design tools used in real-world semiconductor development.

---

# Workshop Summary

## Day 1: Inception of Open-Source EDA, OpenLANE and SKY130 PDK

The first day introduced the ASIC design flow and the open-source ecosystem that enables complete chip design using freely available tools. I explored the OpenLANE automated RTL-to-GDSII flow, the SKY130 Process Design Kit, and the role of various EDA tools involved in synthesis, placement, routing, timing analysis, and verification.

Key concepts learned:

* Introduction to ASIC design methodology
* Open-source EDA toolchain overview
* OpenLANE flow architecture
* SKY130 Process Design Kit (PDK)
* Standard cell libraries
* Design preparation and synthesis flow
* Understanding generated reports and statistics

This day established the foundation required for the remaining stages of the physical design flow.

---

## Day 2: Good Floorplan versus Bad Floorplan and Library Cell Design

The second day focused on floorplanning and placement fundamentals. I learned how proper floorplanning directly affects routing quality, timing closure, congestion, and overall chip performance.

The session covered:

* Floorplan generation
* Utilization factor and aspect ratio
* Power planning concepts
* Placement strategies
* Standard cell architecture
* Library characterization basics
* Cell dimensions and track alignment

By analyzing different floorplanning approaches, I gained insight into how early physical design decisions significantly influence downstream implementation stages.

---

## Day 3: Design Library Cell using Magic Layout and Characterization

The third day concentrated on custom standard cell design using Magic VLSI. A CMOS inverter was manually designed, verified, and characterized to understand transistor-level layout implementation.

Major activities included:

* CMOS inverter layout creation
* Design Rule Checking (DRC)
* Layout versus Schematic (LVS) concepts
* SPICE extraction
* Switching threshold analysis
* Noise margin evaluation
* Rise and fall transition analysis
* Library characterization concepts

This stage provided practical exposure to the relationship between transistor geometry, electrical behavior, and physical implementation.

---

## Day 4: Pre-Layout Timing Analysis and Importance of Clock Tree Synthesis

Day four focused on timing analysis and clock network implementation. I explored how timing is evaluated before routing and how clock distribution affects circuit performance.

Topics covered:

* Static Timing Analysis (STA)
* Setup and Hold timing requirements
* Timing paths and slack calculation
* Clock latency and clock uncertainty
* Clock skew analysis
* Clock Tree Synthesis (CTS)
* Buffer insertion techniques
* Timing optimization strategies

This session highlighted the importance of maintaining synchronized clock distribution and achieving timing closure in high-performance digital designs.

---

## Day 5: Routing, Design Rule Checking and Parasitic Extraction

The final day covered the routing stage, where logical connectivity is transformed into physical metal interconnections. Detailed routing methodologies and verification techniques were explored using modern open-source routing tools.

Key topics included:

* Maze Routing using Lee's Algorithm
* Global Routing and Detailed Routing
* TritonRoute architecture and workflow
* Preprocessed Route Guides
* Access Points (AP) and Access Point Clusters (APC)
* Routing Topology Optimization
* Design Rule Checking (DRC)
* Parasitic Resistance and Capacitance Extraction
* Post-route verification concepts

This stage demonstrated how routing quality directly impacts timing, signal integrity, power consumption, and manufacturability.

---

# Key Learning Outcomes

Throughout this workshop, I gained practical experience with:

* RTL-to-GDSII ASIC implementation flow
* OpenLANE physical design framework
* SKY130 Process Design Kit
* Standard cell design and characterization
* Timing analysis and optimization
* Clock Tree Synthesis
* Physical routing methodologies
* Design Rule Verification
* Layout implementation techniques
* Parasitic extraction and signoff concepts

The workshop provided a complete understanding of how a digital design progresses from RTL description to a fabrication-ready layout while satisfying timing, area, power, and manufacturability requirements.

---

# Conclusion

This repository serves as a comprehensive record of my exploration of the ASIC Physical Design flow using open-source tools and technologies. By progressing through synthesis, floorplanning, placement, characterization, timing analysis, clock tree synthesis, routing, verification, and parasitic extraction, I developed a deeper understanding of the challenges involved in modern VLSI implementation. The knowledge and practical experience gained through this workshop form a strong foundation for further work in ASIC Physical Design, VLSI Design, and Semiconductor Engineering.

