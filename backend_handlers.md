### Prerequisites
Make sure you have the pg package installed for interacting with PostgreSQL:

```
npm install pg
```

And your database pool configuration in a db.js file:

```
const { Pool } = require('pg');
const pool = new Pool({
  user: 'yourUsername',
  host: 'yourRDSInstanceEndpoint',
  database: 'yourDatabaseName',
  password: 'yourPassword',
  port: 5432,
});

module.exports = pool;
```

#### Express Route Handlers
### 1. User Authentication

### 2. Create Flashcard Set
```
app.post('/flashcard-sets', async (req, res) => {
  const { user_id, title, description, is_public } = req.body;
  const query = 'INSERT INTO flashcard_sets (user_id, title, description, is_public) VALUES ($1, $2, $3, $4) RETURNING *';
  try {
    const { rows } = await pool.query(query, [user_id, title, description, is_public]);
    res.status(201).json(rows[0]);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }

});
```
### 3. Fetch User Flashcard Sets
Assuming get_user_flashcard_sets function is created as per previous instructions:

```
app.get('/user/:userId/flashcard-sets', async (req, res) => {
  const { userId } = req.params;
  const query = 'SELECT * FROM flashcard_sets WHERE user_id = $1';
  try {
    const { rows } = await pool.query(query, [userId]);
    res.json(rows);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
```
### 4. Update Flashcard Set
```
app.put('/flashcard-sets/:setId', async (req, res) => {
  const { setId } = req.params;
  const { title, description, is_public } = req.body;
  const query = 'UPDATE flashcard_sets SET title = $2, description = $3, is_public = $4 WHERE set_id = $1 RETURNING *';
  try {
    const { rows } = await pool.query(query, [setId, title, description, is_public]);
    res.json(rows[0]);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
```
### 5. Delete Flashcard Set

```
app.delete('/flashcard-sets/:setId', async (req, res) => {
  const { setId } = req.params;
  const query = 'DELETE FROM flashcard_sets WHERE set_id = $1';
  try {
    await pool.query(query, [setId]);
    res.status(204).send();
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
```
### 6. Add Flashcard to Set

```
app.post('/flashcards', async (req, res) => {
  const { set_id, front_text, back_text, image_url } = req.body;
  const query = 'INSERT INTO flashcards (set_id, front_text, back_text, image_url) VALUES ($1, $2, $3, $4) RETURNING *';
  try {
    const { rows } = await pool.query(query, [set_id, front_text, back_text, image_url]);
    res.status(201).json(rows[0]);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
```
### 7. Fetch Flashcards in Set
Assuming get_flashcards_in_set function is created as per previous instructions:

```
app.get('/flashcard-sets/:setId/flashcards', async (req, res) => {
  const { setId } = req.params;
  const query = 'SELECT * FROM flashcards WHERE set_id = $1';
  try {
    const { rows } = await pool.query(query, [setId]);
    res.json(rows);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
```
### 8. Record Study Session

```
app.post('/study-sessions', async (req, res) => {
  const { user_id, set_id, started_at, ended_at, score } = req.body;
  const query = 'INSERT INTO study_sessions (user_id, set_id, started_at, ended_at, score) VALUES ($1, $2, $3, $4, $5) RETURNING *';
  try {
    const { rows } = await pool.query(query, [user_id, set_id, started_at, ended_at, score]);
    res.status(201).json(rows[0]);
  } catch (error) {
    console.error(error);
    res.status(500).send('Server error');
  }
});
'''
