# FinalProject
Final Project- Directory

from breezypythongui import EasyFrame

import json

import datetime

class PhoneBook(EasyFrame):

    run = True

    def __init__(self):
        #initalizes and adds components to window
        EasyFrame.__init__(self, title = "Phone Book", width=800, height=800)
        self.setBackground("navy")

        self.addLabel(text = "Name: ", row=0 , column=0)
        self.addLabel(text = "Phone Number(No Spaces): ", row=1 , column=0)
        self.addLabel(text = "Address: ", row=2 , column=0)
        

        self.name = self.addTextField(text="", row=0, column=1)
        self.number = self.addTextField(text="", row=1, column=1)
        self.address = self.addTextField(text="", row=2, column=1)

        self.addButton(text="Add Contact", row=3, column=0, command=self.addContact)
        self.addButton(text="Print Contact List", row=3, column=1, command=self.printList)
        self.addButton(text="Get Contact By Name", row=3, column=2, command=self.findName)
        self.addButton(text="Save List To File", row=3, column=3, command=self.saveAsFile)
        self.output = self.addTextArea("",row=0,rowspan=2,column=3,columnspan=2, width=30, height=30) 

        self.contacts={}

    def addContact(self):
            
            name = (self.name.getText())
            number = self.number.getText()
            address = self.address.getText()
            #adds contact to dictionary
            self.contacts.update({name:[number,address]})
            self.messageBox(message="Contact Added!")
            #clears text box
            self.name.setText("")
            self.number.setText("")
            self.address.setText("")
            


    def printList(self):
        #sets text are to contact list
        self.output.setText(json.dumps(self.contacts, indent=4))



    def findName(self):
        #if the name exists, print
        name = (self.name.getText())
        if name in self.contacts:
            self.output.setText(self.contacts.get(name))
        else:
            self.messageBox(title="Error!", message="Name not found in contacts")

    def saveAsFile(self):
        #saves dict as file w/ date and time
        time = str(datetime.datetime.now())
        text = str(self.contacts)
        file = open("contacts.txt", 'w')
        file.write(time)
        file.write(text)
        file.close()



def main():
    PhoneBook().mainloop()

if __name__=="__main__":    main()
