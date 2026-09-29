# an-calculator
a simple calculator for calculation
import tkinter as tk
import math

# ---------------- Calculator Functions ----------------

def press(value):
    entry.insert(tk.END, value)

def clear():
    entry.delete(0, tk.END)

def delete():
    current = entry.get()
    entry.delete(0, tk.END)
    entry.insert(0, current[:-1])

def calculate():
    try:
        expression = entry.get()

        # Replace calculator symbols with Python symbols
        expression = expression.replace("×", "*")
        expression = expression.replace("÷", "/")
        expression = expression.replace("^", "**")
        expression = expression.replace("π", str(math.pi))

        result = eval(expression, {
            "__builtins__": None,
            "sqrt": math.sqrt,
            "sin": lambda x: math.sin(math.radians(x)),
            "cos": lambda x: math.cos(math.radians(x)),
            "tan": lambda x: math.tan(math.radians(x)),
            "log": math.log10,
            "ln": math.log,
            "exp": math.exp
        })

        entry.delete(0, tk.END)
        entry.insert(0, str(result))

    except:
        entry.delete(0, tk.END)
        entry.insert(0, "Error")


def scientific(function):
    try:
        value = float(entry.get())

        if function == "sqrt":
            result = math.sqrt(value)

        elif function == "sin":
            result = math.sin(math.radians(value))

        elif function == "cos":
            result = math.cos(math.radians(value))

        elif function == "tan":
            result = math.tan(math.radians(value))

        elif function == "log":
            result = math.log10(value)

        elif function == "ln":
            result = math.log(value)

        elif function == "square":
            result = value ** 2

        elif function == "factorial":
            result = math.factorial(int(value))

        entry.delete(0, tk.END)
        entry.insert(0, str(result))

    except:
        entry.delete(0, tk.END)
        entry.insert(0, "Error")


# ---------------- Main Window ----------------

root = tk.Tk()
root.title("Scientific Calculator")
root.geometry("430x650")
root.resizable(False, False)

# Dark theme colors
BG = "#121212"
DISPLAY = "#1E1E1E"
BUTTON = "#2A2A2A"
SCI_BUTTON = "#333333"
OPERATOR = "#FF9500"
EQUAL = "#00A86B"
TEXT = "#FFFFFF"

root.configure(bg=BG)

# ---------------- Display ----------------

entry = tk.Entry(
    root,
    font=("Arial", 28),
    bg=DISPLAY,
    fg=TEXT,
    insertbackground=TEXT,
    justify="right",
    bd=0
)

entry.pack(
    padx=15,
    pady=20,
    fill="x",
    ipady=15
)

# ---------------- Button Helper ----------------

def make_button(text, row, column, command,
                color=BUTTON, colspan=1):

    button = tk.Button(
        root,
        text=text,
        command=command,
        font=("Arial", 14, "bold"),
        bg=color,
        fg=TEXT,
        activebackground="#444444",
        activeforeground=TEXT,
        bd=0,
        width=5,
        height=2
    )

    button.grid(
        row=row,
        column=column,
        columnspan=colspan,
        padx=5,
        pady=5,
        sticky="nsew"
    )

# ---------------- Scientific Buttons ----------------

make_button("sin", 1, 0, lambda: scientific("sin"), SCI_BUTTON)
make_button("cos", 1, 1, lambda: scientific("cos"), SCI_BUTTON)
make_button("tan", 1, 2, lambda: scientific("tan"), SCI_BUTTON)
make_button("√", 1, 3, lambda: scientific("sqrt"), SCI_BUTTON)

make_button("log", 2, 0, lambda: scientific("log"), SCI_BUTTON)
make_button("ln", 2, 1, lambda: scientific("ln"), SCI_BUTTON)
make_button("x²", 2, 2, lambda: scientific("square"), SCI_BUTTON)
make_button("x!", 2, 3, lambda: scientific("factorial"), SCI_BUTTON)

# ---------------- Calculator Buttons ----------------

make_button("C", 3, 0, clear, "#C0392B")
make_button("⌫", 3, 1, delete, SCI_BUTTON)
make_button("(", 3, 2, lambda: press("("), SCI_BUTTON)
make_button(")", 3, 3, lambda: press(")"), SCI_BUTTON)

make_button("7", 4, 0, lambda: press("7"))
make_button("8", 4, 1, lambda: press("8"))
make_button("9", 4, 2, lambda: press("9"))
make_button("÷", 4, 3, lambda: press("÷"), OPERATOR)

make_button("4", 5, 0, lambda: press("4"))
make_button("5", 5, 1, lambda: press("5"))
make_button("6", 5, 2, lambda: press("6"))
make_button("×", 5, 3, lambda: press("×"), OPERATOR)

make_button("1", 6, 0, lambda: press("1"))
make_button("2", 6, 1, lambda: press("2"))
make_button("3", 6, 2, lambda: press("3"))
make_button("-", 6, 3, lambda: press("-"), OPERATOR)

make_button("0", 7, 0, lambda: press("0"))
make_button(".", 7, 1, lambda: press("."))
make_button("π", 7, 2, lambda: press("π"))
make_button("+", 7, 3, lambda: press("+"), OPERATOR)

make_button("^", 8, 0, lambda: press("^"), SCI_BUTTON)
make_button("=", 8, 1, calculate, EQUAL, colspan=3)

# Make columns responsive
for i in range(4):
    root.grid_columnconfigure(i, weight=1)

root.mainloop()