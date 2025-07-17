# p2p-backend
// utils/api.js
import { getToken, saveToken, removeToken } from './auth';

const API_URL = 'http://localhost:3000';

async function request(path, options = {}) {
  const token = await getToken();

  const headers = {
    'Content-Type': 'application/json',
    ...(options.headers || {}),
  };

  if (token) {
    headers['Authorization'] = `Bearer ${token}`;
  }

  const res = await fetch(`${API_URL}${path}`, {
    ...options,
    headers,
  });

  if (res.status === 401) {
    // Token ungültig oder abgelaufen
    await removeToken();
    throw new Error('Nicht autorisiert. Bitte neu anmelden.');
  }

  const data = await res.json();
  if (!res.ok) {
    throw new Error(data.message || 'Fehler bei der Anfrage');
  }
  return data;
}

export async function get(path) {
  return request(path, { method: 'GET' });
}

export async function post(path, body) {
  return request(path, { method: 'POST', body: JSON.stringify(body) });
}
// utils/api.js
import { getToken, saveToken, removeToken } from './auth';

const API_URL = 'http://localhost:3000';

async function request(path, options = {}) {
  const token = await getToken();

  const headers = {
    'Content-Type': 'application/json',
    ...(options.headers || {}),
  };

  if (token) {
    headers['Authorization'] = `Bearer ${token}`;
  }

  const res = await fetch(`${API_URL}${path}`, {
    ...options,
    headers,
  });

  if (res.status === 401) {
    // Token ungültig oder abgelaufen
    await removeToken();
    throw new Error('Nicht autorisiert. Bitte neu anmelden.');
  }

  const data = await res.json();
  if (!res.ok) {
    throw new Error(data.message || 'Fehler bei der Anfrage');
  }
  return data;
}

export async function get(path) {
  return request(path, { method: 'GET' });
}

export async function post(path, body) {
  return request(path, { method: 'POST', body: JSON.stringify(body) });
}
// utils/api.js
import { getToken, saveToken, removeToken } from './auth';

const API_URL = 'http://localhost:3000';

async function request(path, options = {}) {
  const token = await getToken();

  const headers = {
    'Content-Type': 'application/json',
    ...(options.headers || {}),
  };

  if (token) {
    headers['Authorization'] = `Bearer ${token}`;
  }

  const res = await fetch(`${API_URL}${path}`, {
    ...options,
    headers,
  });

  if (res.status === 401) {
    // Token ungültig oder abgelaufen
    await removeToken();
    throw new Error('Nicht autorisiert. Bitte neu anmelden.');
  }

  const data = await res.json();
  if (!res.ok) {
    throw new Error(data.message || 'Fehler bei der Anfrage');
  }
  return data;
}

export async function get(path) {
  return request(path, { method: 'GET' });
}

export async function post(path, body) {
  return request(path, { method: 'POST', body: JSON.stringify(body) });
}

npm install nodemailer uuid
MONGO_URI=dein_mongodb_atlas_connection_string
JWT_SECRET=dein_geheimer_wert
EMAIL_USER=deine@gmail.com
EMAIL_PASS=dein_app_passwort
https://p2p-backend.onrender.com/auth/test
export const API_URL = 'https://p2p-backend.onrender.com'; // Render-URL hier eintragen
expo build:android
git clone https://github.com/p2p-payments/backend.git
cd backend
mongodb+srv://USER:PASS@cluster0.xyz.mongodb.net/...)
# Erstelle eine .env-Datei im Backend-Ordner
cp .env.example .env
MONGO_URI=mongodb+srv://DEIN_USER:DEIN_PASS@cluster0.xyz.mongodb.net/p2p-db?retryWrites=true&w=majority
JWT_SECRET=ein_geheimer_string
npm install
npm start
