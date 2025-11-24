#cd "C:\Users\UTC account\Desktop\Personal statement"
#streamlit run myapp.py

import streamlit as st
import pandas as pd
import matplotlib.pyplot as plt
import numpy as np
import math


itMAX = 15

st.markdown("""
# L-system fractal generator
""")
st.write("Finlay A")
st.divider()
st.write("L-systems (Lindenmayer systems) are a mathematical way of describing fractals using simple rules. "
    "Starting with an initial string (axiom), we repeatedly apply replacement rules to generate complex, "
    "self-similar patterns found in nature, for example; plants, trees, and snowflakes. "
    "Try the pre-made fractals below or create your own."
)
st.divider()


def genLsystem(axiom, rules, iterations):
    ax = axiom
    for i in range(iterations):
        new_ax = []
        for c in ax:
            new_ax.append(rules.get(c,c))
        ax = "".join(new_ax)
    return ax


def fractal_dimension(name):
    dimensions = {
        "Koch": round(math.log(4) / math.log(3), 4),  
        "Sierpinski": round(math.log(3) / math.log(2), 4), 
        "Dragon": 2.0,
        "Hilbert": 2.0,
        "Peano": 2.0,
        "Levy": round(math.log(2) / math.log(math.sqrt(2)), 4),  
        "Tree": "~1.7 (varies)"
    }
    return dimensions.get(name, "N/A")



tab1,tab2,tab3,tab4,tab5,tab6,tab7,tab8 = st.tabs(["Custom L-system","Koch curve","Fractal plant","Dragon curve","Sierpinski triangle","Hilbert curve","Peano curve","Levy C curve"])


def Lsystem2segments(commands, step, angle_degree, variables):
    x,y = 0.0, 0.0
    direction = 0.0
    stack = []
    segments= []
    
    for c in commands:
        if c in variables:
            rad = math.radians(direction)
            n_x = x + step * math.cos(rad)
            n_y = y + step * math.sin(rad)
            segments.append(((x,y),(n_x,n_y)))
            x,y = n_x, n_y
       
        elif c =="f":         
            rad = math.radians(direction)
            n_x = x + step * math.cos(rad)
            n_y = y + step * math.sin(rad)
            x,y = n_x, n_y

        elif c == "+":
            direction += angle_degree
        
        elif c == "-":
            direction -= angle_degree

        elif c == "[":
            stack.append((x,y,direction))

        elif c == "]":
            if stack:
                x, y, direction = stack.pop() 
        
    return segments



with tab1:

    st.caption("Define an axiom and the rules that determine how each character (variable) is replaced in each iteration")
    with st.expander("Customise your fractal"):
        st.caption("Available characters are 'ABFGQXY01+-[]'")
        axiom = st.text_input("Enter axiom, initial string",placeholder="e.g  X or FF+")
        rules = {
            }        
        variables = st.multiselect("Select variables, the symbols that draw lines",["A","B","F","G","Q","X","Y","0","1"]) 
        for v in variables:
            if v:
                rule = st.text_input(f"Create your production rule for the symbol {v}:", placeholder=f"e.g {v}{v}+[{v}-]",help="Look at the premade fractals for inspiration")
            if rule:
                st.caption(f"This means: every '{v}' --> '{rule}'")
            meets = True
            valid = ("+-FGBXQA[]01YG")
            for i in rule:
                if i not in valid:
                    meets = False
                
            if rule and meets:
                rules[v] = rule
            elif rule:
                st.warning(f"Invalid characters in rule for '{v}'. Use only: + - F G B X Q A [ ] 0 1 Y")
        angle_degree = st.slider("Enter rotation angle in degrees",0,360,90,step=5,help="How many degrees to turn for '+' (right) and '-' (left) symbols")
        st.caption("Try 60° for hexagonal patterns, 90° for square patterns, or 25° for organic shapes")
        st.write("Rules:",rules)
        


    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX)
    step = 5
    
    if axiom and rules:
        commands = genLsystem(axiom, rules, iterations)
        segments = Lsystem2segments(commands,step,angle_degree,variables)

        fig, axes = plt.subplots()

        for (x1,y1),(x2,y2) in segments:
            axes.plot([x1,x2],[y1,y2])

        axes.set_aspect("equal", "box")
        axes.axis("off")
        
        st.pyplot(fig)

        st.divider()
        st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
        st.write(f"**Total lines:** {len(segments):,}")
    else:
        st.info("Enter an axiom and create at least one rule to generate your fractal")
    with st.expander("Fractal not generating?"):
        st.write("""
    **Common issues and solutions:**
    
    **1. No fractal appears:**
    - Make sure you've entered an axiom (starting string)
    - Ensure at least one rule is created
    - Check that your selected variables match the letters in your rules
    
    **2. Rules not working:**
    - Use only valid characters: `A B F G Q X Y 0 1 + - [ ]`
    - Make sure brackets are balanced: every `[` needs a matching `]`
    - Variables must be selected in the multiselect above
    
    **3. Strange or unexpected shapes:**
    - Start with 1-2 iterations to see the basic pattern
    - Try standard angles: 60°, 90°, 120° for symmetric patterns
    - Check your rule doesn't have unmatched brackets
    
    **4. App running slow:**
    - High iterations (6+) create exponentially more segments
    - Reduce iterations if rendering takes too long
    - Complex rules with many symbols grow faster
    
    **Quick Test:**
    - Axiom: `F`
    - Variable: `F`
    - Rule: `F+F-F-F+F`
    - Angle: `90`
    - Iterations: `3`
    """)

with tab2:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="koch")
    step = 5
    axiom = "F"
    rules = {"F":"F+F-F-F+F"}
    angle_degree = 90
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Koch')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")


with tab3:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="tree")
    step = 5
    axiom = "-X"
    rules = {
        "F":"FF",
        "X":"F+[[X]-]-F[-FX]+X"     
            }
    angle_degree = 25
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F","X"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Tree')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")

with tab4:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="dragon")
    step = 5
    axiom = "F"
    rules = {
        "F":"F+G",
        "G":"F-G"     
            }
    angle_degree = 90
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F","G"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Dragon')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")


with tab5:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="sierpinski")
    step = 5
    axiom = "F-G-G"
    rules = {
        "F":"F-G+F+G-F",
        "G":"GG"     
            }
    angle_degree = 120
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F","G"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Sierpinski')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")

with tab6:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="hilbert")
    step = 5
    axiom = "A"
    rules = {
        "A":"-BF+AFA+FB-",
        "B":"+AF-BFB-FA+"     
            }
    angle_degree = 90
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["A","B","F"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Hilbert')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")

with tab7:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="peano")
    step = 5
    axiom = "F"
    rules = {
        "F":"F+F-F-F-F+F+F+F-F"     
            }
    angle_degree = 90
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Peano')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")

with tab8:
    iterations = st.number_input(f"Iterations, max: {itMAX}",0,itMAX,key="levy")
    step = 5
    axiom = "F"
    rules = {
        "F":"+F--F+"     
            }
    angle_degree = 45
    commands = genLsystem(axiom, rules, iterations)
    segments = Lsystem2segments(commands,step,angle_degree,["F"])

    fig, axes = plt.subplots()

    for (x1,y1),(x2,y2) in segments:
        axes.plot([x1,x2],[y1,y2])

    axes.set_aspect("equal", "box")
    axes.axis("off")
    
    st.pyplot(fig)

    st.divider()
    st.write(f"**Axiom:** `{axiom}`")
    st.write(f"**Angle:** {angle_degree}°")
    st.write("**Rules:**", rules)
    st.write(f"**Generated string (first 100 characters):** `{commands[:100]}`")
    st.write(f"**Total lines:** {len(segments):,}")
    st.write(f"**Fractal Dimension:** {fractal_dimension('Levy')}")
    st.caption("Dimension measures how the fractal fills space: 1 (line) to 2 (plane)")



with st.sidebar:
    st.header("About L-Systems")
    st.write("""
    **Mathematical Foundation:**
    
    L-systems were developed by biologist Aristid Lindenmayer in 1968 to model plant growth.
    
    **Key Concepts:**
    - **Axiom (ω)**: The initial state
    - **Productions (P)**: Rules like F → F+F-F-F+F
    - **Iterations (n)**: Number of times rules are applied
    
    
    **Applications:**
    - Computer graphics
    - Procedural generation
    - Biological modeling
    - Architecture design
    """)
    
    st.divider()
    
    st.header("Fractal Properties")
    st.write("""
    **Self-similarity**: Patterns repeat at different scales
    
    **Fractal Dimension**: A measure of complexity between integer dimensions
    
    **Space-filling**: Some curves (Hilbert, Peano) completely fill 2D space as n→∞
    """)



