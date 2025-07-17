# p2p-backend
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
const express = require('express');
const bcrypt = require('bcrypt');
const jwt = require('jsonwebtoken');
const cors = require('cors');

const app = express();
app.use(express.json());
app.use(cors());

const JWT_SECRET = 'dein_geheimes_jwt_schluessel'; // In Produktion aus Umgebungsvariablen laden

// In-Memory "Datenbank"
const users = {}; // { username: { passwordHash, balance } }

// Middleware zum Verifizieren des JWT
function authenticateToken(req, res, next) {
  const authHeader = req.headers['authorization'];
  const token = authHeader && authHeader.split(' ')[1]; // Bearer TOKEN
  if (!token) return res.sendStatus(401);

  jwt.verify(token, JWT_SECRET, (err, user) => {
    if (err) return res.sendStatus(403);
    req.user = user; // { username: '...' }
    next();
  });
}

// Registrierung
app.post('/register', async (req, res) => {
  const { username, password } = req.body;
  if (users[username]) {
    return res.status(400).json({ message: 'Benutzer existiert bereits' });
  }
  const passwordHash = await bcrypt.hash(password, 10);
  users[username] = { passwordHash, balance: 100 }; // Startguthaben 100
  res.json({ message: 'Benutzer registriert' });
});

// Login
app.post('/login', async (req, res) => {
  const { username, password } = req.body;
  const user = users[username];
  if (!user) return res.status(400).json({ message: 'Benutzer nicht gefunden' });

  const valid = await bcrypt.compare(password, user.passwordHash);
  if (!valid) return res.status(400).json({ message: 'Falsches Passwort' });

  const token = jwt.sign({ username }, JWT_SECRET, { expiresIn: '1h' });
  res.json({ token });
});

// Saldo abfragen (geschützt)
app.get('/balance', authenticateToken, (req, res) => {
  const user = users[req.user.username];
  res.json({ balance: user.balance });
});

// Zahlung senden (geschützt)
app.post('/pay', authenticateToken, (req, res) => {
  const fromUser = req.user.username;
  const { to, amount } = req.body;

  if (!users[to]) return res.status(400).json({ message: 'Empfänger nicht gefunden' });
  if (amount <= 0) return res.status(400).json({ message: 'Ungültiger Betrag' });

  if (users[fromUser].balance < amount) {
    return res.status(400).json({ message: 'Unzureichendes Guthaben' });
  }

  users[fromUser].balance -= amount;
  users[to].balance += amount;

  res.json({
    message: `Erfolgreich ${amount} an ${to} gesendet`,
    fromBalance: users[fromUser].balance,
    toBalance: users[to].balance,
  });
});

app.listen(3000, () => console.log('Server läuft auf Port 3000'));
