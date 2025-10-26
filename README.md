const express = require('express');
const mongoose = require('mongoose');
const cors = require('cors');
const userRoutes = reuire('./rotes/userRoutes');
const app = express();
app.use(cors());
app.use(express.json());

mongoose.connect('mongodb://localhost:27017/javaRestDB')
  .then(() => console.log(' MongoDB connected'))
  .catch(err => console.error(' DB Error:', err));

app.use('/api/users', userRoutes);

const PORT = 5000;
app.listen(PORT, () => console.log(' Server running on port ${PORT}'));
const mongoose = require('mongoose');

const userSchema = new mongoose.Schema({
  name: { type: String, required: true },
  email: String,
  age: Number
});

module.exports = mongoose.model('User', userSchema);
const User = require('../models/userModel');

const getUsers = async (req, res) => {
  const users = await User.find();
  res.json(users);
};

const addUser = async (req, res) => {
  const { name, email, age } = req.body;
  const newUser = new User({ name, email, age });
  await newUser.save();
  res.status(201).json(newUser);
};

const deleteUser = async (req, res) => {
  await User.findByIdAndDelete(req.params.id);
  res.json({ message: 'User deleted successfully' });
};

module.exports = { getUsers, addUser, deleteUser };
const express = require('express');
const { getUsers, addUser, deleteUser } = require('../controllers/userController');
const router = express.Router();

router.get('/', getUsers);
router.post('/', addUser);
router.delete('/:id', deleteUser);

module.exports = router;
