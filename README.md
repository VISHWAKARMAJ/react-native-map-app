# react-native-map-app
import {
  StyleSheet,
  View,
  Image,
  Button,
  Dimensions,
  TextInput,
  SafeAreaView,
  FlatList,
  Text,
} from 'react-native';
import MapView, { Marker } from 'react-native-maps';
import { Component } from 'react/cjs/react.development';

const DATA = [
  {
    id: 'a001',
    name:"expressway charging - mariam enterprise",
    address: 'connaught place, delhi',
    distance:"2102",
    distance_metrics:"metres",
    latitude:"22.4532122",
    longitude:"77.4545322",
    connector_types:["lvlldc-2", "lvl2dc-1","normalac-1"]
  },
  // {
  //   id: 'a002',
  //   name:"expressway charging - mariam enterprise",
  //   address: 'connaught place, delhi',
  //   distance:"2102",
  //   distance_metrics:"metres",
  //   latitude:"22.4532122",
  //   longitude:"77.4545322",
  //   connector_types:["lvlldc-2", "lvl2dc-1","normalac-1"]    
  // }
];

const Item = ({ title }) => (
  <View style={styles.item}>
    <Text style={styles.title}>{title}</Text>
  </View>
);

export default class App extends Component {
  state = {
    snapShot: null,
    markers: [],
  };

  takeSnapshot = () => {
    const snapshot = this.map.takeSnapshot({
      width: 300,
      height: 300,
      format: 'png',
      quality: 0.8,
      result: 'file',
    });
    snapshot.then((uri) => {
      this.setState({
        snapShot: uri,
      });
    });
  };

  onMapPress(e) {
    const lat = e.nativeEvent.coordinate.latitude;
    const lon = e.nativeEvent.coordinate.longitude;
    this.setState({
      markers: [
        ...this.state.markers,
        {
          lat,
          lon,
        },
      ],
    });
  }

  renderItem = ({ item }) => <Item title={item.title} />;

  render() {
    return (
      <SafeAreaView style={styles.container}>
       <View style={{position:"absolute"}}>
        <TextInput plaseceHolder="hello"  />
        </View>
        <MapView
          onPress={(e) => this.onMapPress(e)}
          ref={(map) => {
            this.map = map;
          }}
          style={styles.map}
          initialRegion={{
            latitude: 22.74825,
            longitude: 75.8324,
            latitudeDelta: 0.0922,
            longitudeDelta: 0.0421,
          }}
        >
          {this.state.markers?.map((item) => (
            <Marker
              draggable
              key={0}
              coordinate={{ latitude: item.lat, longitude: item.lon }}
              title={'My Home'}
            />
          ))}
        </MapView>
        <View style={{ borderWidth: 1, position: 'absolute', bottom: 0 }}>
          <Image
            source={{ uri: this.state.snapShot }}
            style={{ height: 200, width: 200 , }}
          />
         
         <Button
            onPress={this.takeSnapshot}
            title="Learn More"
            color="#841584"
            accessibilityLabel="Learn more about this purple button"
          /> 
        
        </View>
        <FlatList
            // horizontal
            data={DATA}
            renderItem={this.renderItem}
            keyExtractor={(item) => item.id}
            style={{height:200, width:200,}}            
          /> 
         
      </SafeAreaView>
      
    );
  }
}

const styles = StyleShe                           et.create({
  container: {
    flex: 1,
    height:20,
    // backgroundColor: '#fff',
    alignItems: 'center',
  },
  map: {
    position: 'absolute',
    width: Dimensions.get('window').width,
    height: Dimensions.get('window').height,
  },
  item: {
    backgroundColor: '#f9c2ff',
    padding: 20,
    marginVertical: 8,
    marginHorizontal: 16,
  },
  title: {
    fontSize: 32,
  },
});
