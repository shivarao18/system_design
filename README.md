# system_design
collection of all system design concepts in a single flowy structure

Module 1, Session 1.1: Introduction to System Design
Welcome to your first session. By the end of this, you'll understand what system design is, why it's a staple in tech interviews, and the crucial difference between high-level and low-level design.

What is System Design?
Imagine you're tasked with building a house. You wouldn't just start buying bricks and mixing cement, right? You'd first create a plan. You'd think about:

How many rooms do you need? (Functional Requirements)
Should it withstand earthquakes or hurricanes? (Non-Functional Requirements)
What's the overall architectural style? (High-Level Design)
Where exactly will the electrical outlets and plumbing go in the kitchen? (Low-Level Design)
System design is the process of creating the architectural plan for a software system.

It involves defining the architecture, components, interfaces, and data for a system that will satisfy a specific set of requirements. The goal is to create a blueprint for a system that is:

Scalable: Can it handle growth in users and data?
Reliable: Does it work correctly and consistently?
Available: Is it up and running when users need it?
Maintainable: Can other engineers easily understand and modify it?
It’s less about writing code and more about making high-level technical decisions and being able to justify them.

Why is it so Important in Interviews?
Interviewers use system design questions to gauge skills that are hard to test in a typical coding challenge. They are trying to see:

Your Thought Process: There is no single "correct" answer. They want to see how you handle ambiguity, break down a large, complex problem into smaller pieces, and make reasonable assumptions.
Technical Knowledge: Do you have a broad understanding of different technologies (databases, caching, load balancing) and their trade-offs?
Communication Skills: Can you clearly articulate your ideas, explain your design choices, and lead a technical discussion?
Real-World Experience: It shows you can think about the big picture, considering constraints like cost, time, and the business goals the system is trying to achieve.
Essentially, they are assessing your potential to be not just a coder, but a future technical leader or architect.

High-Level Design (HLD) vs. Low-Level Design (LLD)
This is a fundamental concept that trips up many beginners. Let's clarify it with our house analogy.

High-Level Design (HLD): The Architect's Blueprint

Focus: The "bird's-eye view" or the macro-level structure of the system.
What it includes: The major components and how they interact. Think of it as a diagram with big boxes and arrows.
Examples of components: Web Servers, Load Balancers, Databases, Caching Layers, Microservices.
The Question it Answers: WHAT are the main building blocks of the system?
Interview Context: This is what 95% of system design interviews are about. When an interviewer asks you to "Design Twitter," they are asking for the HLD.
Example HLD for a simple blog:
A user's browser (Client) talks to a Load Balancer, which distributes traffic to one of several Web Servers, which then read or write data to a Database.

Low-Level Design (LLD): The Room's Detailed Floor Plan

Focus: The "zoomed-in view" or the micro-level implementation within a single component.
What it includes: How the actual code will be structured.
Examples: Class diagrams, object relationships, specific database table schemas (column names, types, indexes), API endpoint definitions (GET /users/{id}).
The Question it Answers: HOW will a specific piece of the system actually work?
Interview Context: This is closer to an "Object-Oriented Design" or a coding interview. For example, "Design the classes for a parking lot system" or "Design the database schema for a user profile."
Summary in a Nutshell:

HLD is about the architecture of the whole system. (Designing the city's road network).
LLD is about the internals of a single component. (Designing the engine of one specific car model).

