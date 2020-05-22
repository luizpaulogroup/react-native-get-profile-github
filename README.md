
```JS
import React, { Component } from 'react';
import { Text, View, TextInput, Image, TouchableOpacity } from 'react-native';

export default class App extends Component {
  state = {
    name: '',
    text: '',
    id: '',
    location: '',
    bio: '',
    avatar_url: '',
    error: false,
  };

  componentDidMount = () => {};

  _handleProfileGithub = async () => {
    const { text } = this.state;

    fetch(`https://api.github.com/users/${text}`)
      .then(res => res.json())
      .then(response => {
        if (response.message == 'Not Found') {
          this.setState({ error: true });
          return;
        }

        const { avatar_url, name, id, location, bio } = response;

        this.setState({ avatar_url, name, id, location, bio, error: false });
      });
  };

  render() {
    const { name, avatar_url, id, location, bio, error } = this.state;

    return (
      <View style={{ padding: 5 }}>
        <TextInput
          style={{
            borderColor: 'purple',
            borderWidth: 2,
            padding: 5,
            marginTop: 10,
            borderRadius: 15,
            justifyContent: 'center',
            alignItems: 'center',
          }}
          placeholder="please, entry your name of github account"
          onChangeText={text => this.setState({ text })}
        />
        <TouchableOpacity
          style={{
            backgroundColor: 'purple',
            padding: 5,
            borderRadius: 15,
            justifyContent: 'center',
            alignItems: 'center',
            marginTop: 5,
          }}
          onPress={() => this._handleProfileGithub()}>
          <Text style={{ color: '#FFF', fontWeight: 'bold' }}>find</Text>
        </TouchableOpacity>
        <View>
          {error ? (
            <View
              style={{
                justifyContent: 'center',
                alignItems: 'center',
                padding: 5,
              }}>
              <Text>account not found!</Text>
            </View>
          ) : (
            <View
              style={{
                justifyContent: 'center',
                alignItems: 'center',
                padding: 5,
              }}>
              <Image
                source={{ uri: avatar_url }}
                style={{
                  width: 100,
                  height: 100,
                  alignSelf: 'center',
                  borderRadius: 100,
                }}
              />
              <Text style={{ marginTop: 5 }}>name: {name}</Text>
              <Text style={{ marginTop: 5 }}>id: {id}</Text>
              <Text style={{ marginTop: 5 }}>location: {location}</Text>
              <Text style={{ marginTop: 5 }}>bio: {bio}</Text>
            </View>
          )}
        </View>
      </View>
    );
  }
}
```
