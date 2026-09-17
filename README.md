# Antigravity Coding Skill Set



A modular engineering skill system designed for AI coding agents, with

Antigravity/Gemini as the primary environment.



The system separates engineering responsibilities into a small orchestration

core and focused specialist skills. The core decides which capabilities a

task actually requires, while specialist skills provide deep domain-specific

guidance.



\## Philosophy



The Antigravity Coding Skill Set follows a simple principle:



> The core decides what engineering capabilities are required; specialist

> skills provide focused expertise for those capabilities.



It is designed to avoid two common problems in AI-assisted development:



1\. One enormous instruction set that is loaded for every task.

2\. Many independent skills activating unnecessarily because of simple keyword

&#x20;  matches.



Instead, the system uses intent, affected system area, evidence, and negative

trigger exclusions to determine which specialists are actually relevant.



\## Architecture



```text

Antigravity Coding Skill Set

â”‚

â”œâ”€â”€ Core

â”‚   â””â”€â”€ Professional Coding Core

â”‚

â”œâ”€â”€ Engineering Specialists

â”‚   â”œâ”€â”€ Architecture

â”‚   â”œâ”€â”€ Debugging

â”‚   â”œâ”€â”€ Testing

â”‚   â”œâ”€â”€ Security

â”‚   â”œâ”€â”€ API Contracts

â”‚   â”œâ”€â”€ Data

â”‚   â”œâ”€â”€ Performance

â”‚   â”œâ”€â”€ Concurrency

â”‚   â”œâ”€â”€ Dependencies

â”‚   â”œâ”€â”€ Refactoring

â”‚   â””â”€â”€ Code Review

â”‚

â””â”€â”€ External Specialist Integration

&#x20;   â””â”€â”€ OpenDesign

&#x20;       â””â”€â”€ UI/UX and visual design


