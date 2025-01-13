import tkinter as tk
from tkinter import messagebox, simpledialog

# Contact list storage
contacts = []

# Add a new contact
def add_contact():
    name = simpledialog.askstring("Add Contact", "Enter Name:")
    if not name:
        messagebox.showerror("Error", "Name cannot be empty!")
        return
    phone = simpledialog.askstring("Add Contact", "Enter Phone Number:")
    email = simpledialog.askstring("Add Contact", "Enter Email Address:")
    address = simpledialog.askstring("Add Contact", "Enter Address:")
    
    contacts.append({"name": name, "phone": phone, "email": email, "address": address})
    messagebox.showinfo("Success", "Contact added successfully!")
    refresh_contact_list()

# View contact list
def refresh_contact_list():
    contact_list.delete(0, tk.END)
    for contact in contacts:
        contact_list.insert(tk.END, f"{contact['name']} - {contact['phone']}")

# Search contact
def search_contact():
    query = simpledialog.askstring("Search Contact", "Enter Name or Phone Number:")
    if not query:
        messagebox.showerror("Error", "Search query cannot be empty!")
        return
    results = [c for c in contacts if query.lower() in c['name'].lower() or query in c['phone']]
    if results:
        message = "\n".join([f"{c['name']} - {c['phone']} - {c['email']} - {c['address']}" for c in results])
        messagebox.showinfo("Search Results", message)
    else:
        messagebox.showinfo("No Results", "No contacts found.")

# Update contact
def update_contact():
    name = simpledialog.askstring("Update Contact", "Enter the name of the contact to update:")
    contact = next((c for c in contacts if c['name'].lower() == name.lower()), None)
    if not contact:
        messagebox.showerror("Error", "Contact not found!")
        return
    new_name = simpledialog.askstring("Update Contact", "Enter New Name:", initialvalue=contact['name'])
    new_phone = simpledialog.askstring("Update Contact", "Enter New Phone Number:", initialvalue=contact['phone'])
    new_email = simpledialog.askstring("Update Contact", "Enter New Email:", initialvalue=contact['email'])
    new_address = simpledialog.askstring("Update Contact", "Enter New Address:", initialvalue=contact['address'])
    
    contact.update({"name": new_name, "phone": new_phone, "email": new_email, "address": new_address})
    messagebox.showinfo("Success", "Contact updated successfully!")
    refresh_contact_list()

# Delete contact
def delete_contact():
    name = simpledialog.askstring("Delete Contact", "Enter the name of the contact to delete:")
    global contacts
    contacts = [c for c in contacts if c['name'].lower() != name.lower()]
    messagebox.showinfo("Success", "Contact deleted successfully!")
    refresh_contact_list()

# Main UI
app = tk.Tk()
app.title("Contact Manager")

frame = tk.Frame(app)
frame.pack(pady=10)

contact_list = tk.Listbox(frame, width=50, height=15)
contact_list.pack(side=tk.LEFT, padx=10)

scrollbar = tk.Scrollbar(frame, orient=tk.VERTICAL, command=contact_list.yview)
scrollbar.pack(side=tk.RIGHT, fill=tk.Y)
contact_list.config(yscrollcommand=scrollbar.set)

button_frame = tk.Frame(app)
button_frame.pack(pady=10)

add_button = tk.Button(button_frame, text="Add Contact", command=add_contact, width=15)
add_button.grid(row=0, column=0, padx=5)

search_button = tk.Button(button_frame, text="Search Contact", command=search_contact, width=15)
search_button.grid(row=0, column=1, padx=5)

update_button = tk.Button(button_frame, text="Update Contact", command=update_contact, width=15)
update_button.grid(row=0, column=2, padx=5)

delete_button = tk.Button(button_frame, text="Delete Contact", command=delete_contact, width=15)
delete_button.grid(row=0, column=3, padx=5)

refresh_contact_list()

app.mainloop()
