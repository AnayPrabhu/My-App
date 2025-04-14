import React, { useState } from 'react';
import { MapContainer, TileLayer, Marker, Popup, useMapEvents } from 'react-leaflet';
import { GeoSearchControl, OpenStreetMapProvider } from 'leaflet-geosearch';
import { useEffect } from 'react';
import 'leaflet/dist/leaflet.css';
import 'leaflet-geosearch/dist/geosearch.css';
import L from 'leaflet';

function SearchControl() {
  const map = useMapEvents({});

  useEffect(() => {
    const provider = new OpenStreetMapProvider();

    const searchControl = new GeoSearchControl({
      provider,
      style: 'bar',
      autoClose: true,
      keepResult: true,
    });

    map.addControl(searchControl);

    return () => map.removeControl(searchControl);
  }, [map]);

  return null;
}

function LocationMarker({ onAddMarker }) {
  useMapEvents({
    click(e) {
      const { lat, lng } = e.latlng;
      const review = prompt("Enter your review:");
      const imageUrl = prompt("Enter image URL (optional):");
      onAddMarker({ lat, lng, review, imageUrl });
    }
  });
  return null;
}

function App() {
  const [markers, setMarkers] = useState([]);

  const handleAddMarker = (marker) => {
    setMarkers((prev) => [...prev, marker]);
  };

  return (
    <div style={{ height: '100vh', width: '100%' }}>
      <MapContainer center={[51.505, -0.09]} zoom={13} style={{ height: '100%', width: '100%' }}>
        <TileLayer
          attribution='&copy; <a href="https://osm.org/copyright">OpenStreetMap</a> contributors'
          url="https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png"
        />
        <SearchControl />
        <LocationMarker onAddMarker={handleAddMarker} />
        {markers.map((marker, idx) => (
          <Marker key={idx} position={[marker.lat, marker.lng]}>
            <Popup>
              <strong>Review:</strong> {marker.review}<br />
              {marker.imageUrl && <img src={marker.imageUrl} alt="User" width="100%" />}
            </Popup>
          </Marker>
        ))}
      </MapContainer>
    </div>
  );
}

export default App;[README.md](https://github.com/user-attachments/files/19735441/README.md)
