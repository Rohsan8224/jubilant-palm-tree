import tkinter as tk
from tkinter import messagebox
from tkinter import ttk


def validate_customer_name(input_text):
    # Validate customer name input
    if all(char.isalpha() or char.isspace() for char in input_text):
        return True
    else:
        messagebox.showerror("Error", "Customer name can only contain alphabetic characters and spaces.")
        return False


def validate_quantity(input_text):
    # Validate quantity input
    if input_text.isdigit() and 1 <= int(input_text) <= 500 or input_text == "":
        return True
    else:
        messagebox.showerror("Error", "Quantity must be a number between 1 and 500.")
        return False


def place_order():
    global receipt_counter
    # Get input values
    name = entry_name.get()
    item = selected_item.get()
    quantity = entry_quantity.get()

    # Validate input
    if not name or not item or not quantity:
        messagebox.showerror("Error", "Please fill in all fields.")
        return

    # Add order to the list
    order = {
        "name": name,
        "item": item,
        "quantity": quantity,
        "receipt_number": receipt_counter
    }
    receipt_counter += 1
    orders.append(order)

    # Update treeview
    tree.insert("", tk.END, text=order["receipt_number"], values=(order["name"], order["item"], order["quantity"]))

    # Clear input fields
    entry_name.delete(0, tk.END)
    entry_quantity.delete(0, tk.END)


def view_receipt():
    # Get the selected item
    selected_item = tree.selection()
    if selected_item:
        # Retrieve the order
        item_id = tree.item(selected_item)["text"]
        order = next((o for o in orders if o["receipt_number"] == int(item_id)), None)
        if order:
            receipt_text = generate_receipt(order)
            messagebox.showinfo("Receipt", receipt_text)


def delete_order():
    # Get the selected item
    selected_item = tree.selection()
    if selected_item:
        # Retrieve the order
        item_id = tree.item(selected_item)["text"]
        order = next((o for o in orders if o["receipt_number"] == int(item_id)), None)
        if order:
            # Remove order from the list
            orders.remove(order)

            # Update the treeview
            tree.delete(selected_item)


def format_order(order):
    return f"Receipt Number: R-{order['receipt_number']}\n" \
           f"Customer: {order['name']}\n" \
           f"Item: {order['item']}\n" \
           f"Quantity: {order['quantity']}\n"


def generate_receipt(order):
    return f"Receipt Number: R-{order['receipt_number']}\n\n" \
           f"Customer: {order['name']}\n" \
           f"Item: {order['item']}\n" \
           f"Quantity: {order['quantity']}\n"


# Create the main window
root = tk.Tk()
root.title("Julie's party hire")

# Set the background color for the treeview
style = ttk.Style()
style.configure("Treeview", background="white")

# Create the treeview
tree = ttk.Treeview(root)
tree["columns"] = ("Name", "Item", "Quantity")
tree.heading("#0", text="Receipt Number")
tree.heading("Name", text="Customer")
tree.heading("Item", text="Item Hired")
tree.heading("Quantity", text="Quantity")
tree.pack(padx=10, pady=10)

# Create a scrollable frame for the treeview
scrollbar = ttk.Scrollbar(root, orient="vertical", command=tree.yview)
scrollbar.pack(side="right", fill="y")
tree.configure(yscrollcommand=scrollbar.set)

# Create a frame for the input fields and buttons
input_frame = tk.Frame(root, bg="white")
input_frame.pack(padx=10, pady=5)

# Customer Name
lbl_name = tk.Label(input_frame, text="Customer Name:")
lbl_name.grid(row=0, column=0, padx=10, pady=5, sticky="w")
entry_name = tk.Entry(input_frame)
entry_name.grid(row=0, column=1, padx=10, pady=5)

# Item Hired
lbl_item = tk.Label(input_frame, text="Item Hired:")
lbl_item.grid(row=1, column=0, padx=10, pady=5, sticky="w")
items = ["Tables", "Chairs", "Decorations", "Sound System", "BBQ"]
selected_item = tk.StringVar(value=items[0])
dropdown_item = ttk.Combobox(input_frame, textvariable=selected_item, values=items, state="readonly")
dropdown_item.grid(row=1, column=1, padx=10, pady=5)

# Quantity
lbl_quantity = tk.Label(input_frame, text="Quantity:")
lbl_quantity.grid(row=2, column=0, padx=10, pady=5, sticky="w")
entry_quantity = tk.Entry(input_frame)
entry_quantity.grid(row=2, column=1, padx=10, pady=5)

# Place Order Button
btn_order = tk.Button(input_frame, text="Place Order", command=place_order, width=20)
btn_order.grid(row=3, column=0, columnspan=2, padx=10, pady=10)

# View Receipt Button
btn_receipt = tk.Button(input_frame, text="View Receipt", command=view_receipt, width=20)
btn_receipt.grid(row=4, column=0, columnspan=2, padx=10, pady=10)

# Delete Button
btn_delete = tk.Button(input_frame, text="Delete Order", command=delete_order, width=20)
btn_delete.grid(row=5, column=0, columnspan=2, padx=10, pady=5)

# Initialize data
orders = []
receipt_counter = 1

# Validate Customer Name Entry
entry_name.config(validate="key", validatecommand=(root.register(validate_customer_name), "%P"))
entry_quantity.config(validate="key", validatecommand=(root.register(validate_quantity), "%P"))

# Run the application
root.mainloop()
