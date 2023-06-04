import React, { useState, useEffect } from 'react';
import { View, Text, FlatList, TextInput, TouchableOpacity, Modal, Alert } from 'react-native';
import Contacts from 'react-native-contacts';

const App = () => {
  const [contacts, setContacts] = useState([]);
  const [searchQuery, setSearchQuery] = useState('');
  const [selectedContact, setSelectedContact] = useState(null);

  useEffect(() => {
    Contacts.getAll((err, contacts) => {
      if (err) {
        throw err;
      }
      setContacts(contacts);
    });
  }, []);

  const handleSearch = (query) => {
    setSearchQuery(query);
  };

  const handleClearSearch = () => {
    setSearchQuery('');
  };

  const handleContactPress = (contact) => {
    setSelectedContact(contact);
  };

  const handleModalDismiss = () => {
    setSelectedContact(null);
  };

  const handleCallContact = (contact) => {
    Alert.alert(`Calling ${contact.givenName} ${contact.familyName}...`);
  };

  const handleMessageContact = (contact) => {
    Alert.alert(`Messaging ${contact.givenName} ${contact.familyName}...`);
  };

  const handleEditContact = (contact) => {
    Alert.alert(`Editing ${contact.givenName} ${contact.familyName}...`);
  };

  const handleAddContact = () => {
    Alert.alert('Adding new contact...');
  };

  const handleDeleteContact = (contact) => {
    Alert.alert(`Deleting ${contact.givenName} ${contact.familyName}...`);
  };

  const handleEditContactList = (contact) => {
    Alert.alert(`Editing ${contact.givenName} ${contact.familyName}...`);
  };

  const filteredContacts = contacts.filter((contact) =>
    contact.givenName.toLowerCase().includes(searchQuery.toLowerCase())
  );

  return (
    <View>
      <View style={{ flexDirection: 'row', alignItems: 'center' }}>
        <TextInput
          style={{ flex: 1 }}
          placeholder="Search contacts"
          value={searchQuery}
          onChangeText={handleSearch}
        />
        <TouchableOpacity onPress={handleClearSearch}>
          <Text>Clear</Text>
        </TouchableOpacity>
      </View>
      <FlatList
        data={filteredContacts}
        keyExtractor={(item) => item.recordID}
        renderItem={({ item }) => (
          <TouchableOpacity onPress={() => handleContactPress(item)}>
            <Text>{item.givenName} {item.familyName}</Text>
            <Text>{item.phoneNumbers[0].number}</Text>
            <View style={{ flexDirection: 'row', justifyContent: 'flex-end' }}>
              <TouchableOpacity onPress={() => handleEditContactList(item)}>
                <Text>Edit</Text>
              </TouchableOpacity>
              <TouchableOpacity onPress={() => handleDeleteContact(item)}>
                <Text>Delete</Text>
              </TouchableOpacity>
            </View>
          </TouchableOpacity>
        )}
      />
      <TouchableOpacity onPress={handleAddContact}>
        <Text>Add contact</Text>
      </TouchableOpacity>
      <Modal visible={selectedContact !== null} onRequestClose={handleModalDismiss}>
        <View>
          <Text>{selectedContact?.givenName} {selectedContact?.familyName}</Text>
          <Text>{selectedContact?.phoneNumbers[0].number}</Text>
          <View style={{ flexDirection: 'row', justifyContent: 'space-between' }}>
            <TouchableOpacity onPress={() => handleCallContact(selectedContact)}>
              <Text>Call</Text>
            </TouchableOpacity>
            <TouchableOpacity onPress={() => handleMessageContact(selectedContact)}>
              <Text>Message</Text>
            </TouchableOpacity>
            <TouchableOpacity onPress={() => handleEditContact(selectedContact)}>
              <Text>Edit</Text>
            </TouchableOpacity>
          </View>
          <TouchableOpacity onPress={handleModalDismiss}>
            <Text>Dismiss</Text>
          </TouchableOpacity>
        </View>
      </Modal>
    </View>
  );
};
