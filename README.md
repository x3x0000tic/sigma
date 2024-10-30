# sigma

import React, { useState, useEffect } from 'react';
import { Text, View, StyleSheet, Image, TouchableOpacity, Alert, TextInput } from 'react-native';
import * as ImagePicker from 'expo-image-picker';
import * as Sharing from 'expo-sharing';

const App = () => {
  const [imageUri, setImageUri] = useState('https://cdn-icons-png.flaticon.com/512/1077/1077012.png'); // Nueva imagen
  // Nueva imagen

  useEffect(() => {
    (async () => {
      const cameraStatus = await ImagePicker.requestCameraPermissionsAsync();
      const libraryStatus = await ImagePicker.requestMediaLibraryPermissionsAsync();
      if (cameraStatus.status !== 'granted' || libraryStatus.status !== 'granted') {
        Alert.alert("Permisos denegados", "Se requieren permisos para acceder a la cámara y galería.");
      }
    })();
  }, []);

  const pickImageGaleria = async () => {
    try {
      const result = await ImagePicker.launchImageLibraryAsync({
        mediaTypes: ImagePicker.MediaTypeOptions.Images,
        allowsEditing: true,
        aspect: [4, 3],
        quality: 1,
      });

      if (!result.canceled && result.assets && result.assets[0].uri) {
        setImageUri(result.assets[0].uri);
      } else {
        Alert.alert("Selección cancelada", "No se seleccionó ninguna imagen.");
      }
    } catch (error) {
      console.error("Error al seleccionar imagen de la galería:", error);
    }
  };

  const pickImageFoto = async () => {
    try {
      const result = await ImagePicker.launchCameraAsync({
        allowsEditing: true,
        aspect: [4, 3],
        quality: 1,
      });

      if (!result.canceled && result.assets && result.assets[0].uri) {
        setImageUri(result.assets[0].uri);
      } else {
        Alert.alert("Captura cancelada", "No se tomó ninguna foto.");
      }
    } catch (error) {
      console.error("Error al tomar la foto:", error);
    }
  };

  const shareImage = async () => {
    if (!imageUri) {
      Alert.alert("Error", "No hay ninguna imagen para compartir.");
      return;
    }

    try {
      await Sharing.shareAsync(imageUri);
    } catch (error) {
      console.error("Error al compartir imagen:", error);
      Alert.alert("Error al compartir", "No se pudo compartir la imagen.");
    }
  };

  return (
    <View style={styles.container}>
      <View style={styles.subcontainer}>
        <Text style={styles.title}>Inicio de Sesion</Text>
        
        <TouchableOpacity onPress={pickImageGaleria}>
          <Image source={{ uri: imageUri }} style={styles.image} />
        </TouchableOpacity>

        <TouchableOpacity onPress={shareImage} style={styles.button3}>
          <Text style={styles.buttonText}>COMPARTIR</Text>
        </TouchableOpacity>

        <TouchableOpacity style={styles.button2} onPress={pickImageFoto}>
          <Text style={styles.buttonText}>TOMAR UNA FOTO</Text>
        </TouchableOpacity>

        <View style={styles.subcontainer2}>
          <Text style={styles.subtitle}>Nombre de usuario:</Text>
          <TextInput style={styles.input} placeholder="Nombre" />

          <Text style={styles.subtitle}>Contraseña:</Text>
          <TextInput style={styles.input} placeholder="Contraseña" secureTextEntry={true} />
        </View>

        <TouchableOpacity style={styles.button} onPress={() => Alert.alert('Usuario Registrado')}>
          <Text style={styles.buttonText}>ACEPTAR</Text>
        </TouchableOpacity>
      </View>
    </View>
  );
};

const styles = StyleSheet.create({
  container: {
    flex: 1,
    backgroundColor: '#e0f7fa', // Fondo aqua suave
    alignItems: 'center',
    justifyContent: 'flex-start',
    paddingTop: 50,
  },
  subcontainer2: {
    marginTop: 20,
    marginBottom: 20,
    paddingHorizontal: 15,
  },
  subcontainer: {
    borderColor: '#ff8a65', // Naranja para el borde
    backgroundColor: '#ffffff',
    borderWidth: 1,
    alignItems: 'center',
    justifyContent: 'center',
    padding: 25,
    borderRadius: 15,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.2,
    shadowRadius: 6,
    elevation: 5,
  },
  title: {
    fontSize: 26,
    fontFamily: 'Arial',
    fontWeight: '800',
    color: '#ff6f61', // Rojo coral
    textAlign: 'center',
  },
  subtitle: {
    fontSize: 16,
    fontFamily: 'Arial',
    color: '#00838f', // Azul oscuro
    marginTop: 8,
    textAlign: 'center',
  },
  image: {
    height: 160,
    width: 160,
    borderRadius: 15,
    marginTop: 20,
    marginBottom: 20,
    borderColor: '#ff8a65',
    borderWidth: 2,
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 4 },
    shadowOpacity: 0.2,
    shadowRadius: 5,
    elevation: 5,
  },
  input: {
    padding: 10,
    height: 40,
    width: 250,
    borderRadius: 8,
    backgroundColor: '#ffffff',
    color: '#00838f',
    marginTop: 12,
    marginBottom: 20,
    borderColor: '#cfd8dc',
    borderWidth: 1,
    fontFamily: 'Arial',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 2 },
    shadowOpacity: 0.1,
    shadowRadius: 4,
    elevation: 2,
  },
  button: {
    height: 45,
    width: 120,
    backgroundColor: "#ff8a65",
    borderRadius: 10,
    borderColor: '#ff7043',
    borderWidth: 1,
    justifyContent: "center",
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 3 },
    shadowOpacity: 0.2,
    shadowRadius: 5,
    elevation: 4,
  },
  button2: {
    height: 45,
    width: 140,
    color: '#ffffff',
    backgroundColor: "#81d4fa", // Azul claro
    borderRadius: 10,
    borderColor: '#4fc3f7', // Azul más oscuro
    borderWidth: 1,
    justifyContent: "center",
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 3 },
    shadowOpacity: 0.2,
    shadowRadius: 5,
    elevation: 4,
  },
  button3: {
    padding: 10,
    marginBottom: 15,
    height: 45,
    width: 120,
    backgroundColor: "#ffcc80", // Naranja claro
    borderRadius: 10,
    borderColor: '#ffb74d', // Naranja oscuro
    borderWidth: 1,
    justifyContent: "center",
    alignItems: 'center',
    shadowColor: '#000',
    shadowOffset: { width: 0, height: 3 },
    shadowOpacity: 0.2,
    shadowRadius: 5,
    elevation: 4,
  },
  buttonText: {
    color: "#ffffff",
    fontSize: 14,
    fontWeight: '600',
    textAlign: "center"
  }
});

export default App;
