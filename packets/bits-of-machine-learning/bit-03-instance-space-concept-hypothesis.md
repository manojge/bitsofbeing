<!--
Title: Bit #03 — Instance Space, Concepts, and Hypotheses
Description: Understanding how machine learning searches for rules that generalize across an enormous instance space using limited data.
Author: George Manoj
Date: 2026-04-24
Tags: machine learning, instance space, concept function, hypothesis space, generalization, The Master Algorithm
-->

# Bit #03 — Instance Space, Concepts, and Hypotheses

Machine learning tries to find a rule — a concept function — that correctly labels every possible input in a problem’s instance space.  
The instance space is the universe of all inputs the world could present. In a traffic‑management problem, the instance space includes every possible traffic scenario across all roads, times, weather conditions, and driver behaviors.  
A dataset is only a tiny sample drawn from this vast space — for example, traffic data collected from a single intersection or a few sensors.  
Because we cannot explore the entire instance space, we search within a hypothesis space — the set of rules we are willing to consider.  
Learning is the process of proposing hypotheses, testing them on the dataset, and refining them when they fail.  
A good hypothesis is one that generalizes beyond the data we saw and works on unseen parts of the instance space.  
This pattern appears in history: humanity proposed many hypotheses about motion — from Archimedes to Galileo — until Newton formulated laws that generalized across most physical situations.  
Machine learning follows the same logic: from limited observations, we search for a rule that explains as much of the instance space as possible.  

*(These notes are part of my reading reflections on **The Master Algorithm** by Pedro Domingos.)*

