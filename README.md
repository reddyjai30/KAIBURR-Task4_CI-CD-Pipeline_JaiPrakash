![1583864622173-removebg-preview](https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash/assets/47852931/9f5a6975-67cd-4218-99a8-9e7a9312faa1)

# KAIBURR Coding Assignment
##### Name: Jai Prakash Reddy D 
##### R.no: CB.EN.U4CSE20027
###### Mail: reddyjai30@gmail.com



# Task 3 - WEB UI Forms

## Overview 
 This below gives a broad view to the React Web UI developed for the [Task 1](https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash) using React  and Material-UI, integrated with a Spring Boot backend as we did for the Task 1. The application allows users to create, view, search, and delete tasks       efficiently.
 
------  


## Application Setup

### Prerequisites

- **Node.js and npm**
- **React**
- **Material-UI**
- **Spring Boot (for the backend API)** : We created this from [Task 1](https://github.com/reddyjai30/KAIBURR-Task1_JavaRESTAPI_JaiPrakash)



## Installation
**1. Clone the repository:**
```bash
git clone (https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash)
```
**2. Navigate to the project directory and Install dependencies:**
```bash
npm install
```

## Running the Application
To start the application, run:
```bash
npm start
```
<img width="1000" alt="Screenshot 2024-01-14 at 5 51 47 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/db44a787-3b45-4de2-af90-6ad3e2c24cb4">

The application will be available at `http://localhost:3000`

<img width="1000" alt="Screenshot 2024-01-14 at 5 51 15 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/9ce7a71b-9af5-4650-909e-eb8b0479e12d">


## Application Structure
The application consists of several components, each responsible for different functionalities:

### App.js
Main component that includes routing and layout.
```bash
// App.js
import React from 'react';
import { BrowserRouter as Router, Routes, Route, Link } from 'react-router-dom';
import { Drawer, List, ListItem, ListItemText, AppBar, Toolbar, Typography, Box, CssBaseline } from '@mui/material';
import TaskList from './TaskList';
import TaskForm from './TaskForm';
import SearchByName from './SearchByName';
import SearchByAssignee from './SearchByAssignee';
import HomePage from './HomePage'; // Import the HomePage component

const drawerWidth = 240;

const App = () => {
  return (
    <Router>
      <CssBaseline />
      <AppBar position="fixed" sx={{ zIndex: (theme) => theme.zIndex.drawer + 1 }}>
        <Toolbar>
          <Typography variant="h6" noWrap>
            KAIBURR Task Management
          </Typography>
        </Toolbar>
      </AppBar>
      <Drawer
        variant="permanent"
        sx={{
          width: drawerWidth,
          [`& .MuiDrawer-paper`]: { width: drawerWidth },
        }}
      >
        <Toolbar />
        <Box sx={{ overflow: 'auto' }}>
          <List>
            <ListItem button component={Link} to="/">
              <ListItemText primary="Home" />
            </ListItem>
            <ListItem button component={Link} to="/create-task">
              <ListItemText primary="Create Task" />
            </ListItem>
            <ListItem button component={Link} to="/tasks">
              <ListItemText primary="View Tasks" />
            </ListItem>
            <ListItem button component={Link} to="/search-by-name">
              <ListItemText primary="Search by Name" />
            </ListItem>
            <ListItem button component={Link} to="/search-by-assignee">
              <ListItemText primary="Search by Assignee" />
            </ListItem>
          </List>
        </Box>
      </Drawer>
      <Box component="main" sx={{ flexGrow: 1, p: 3, ml: `${drawerWidth}px` }}>
        <Toolbar />
        <Routes>
          <Route path="/" element={<HomePage />} />
          <Route path="/create-task" element={<TaskForm />} />
          <Route path="/tasks" element={<TaskList />} />
          <Route path="/search-by-name" element={<SearchByName />} />
          <Route path="/search-by-assignee" element={<SearchByAssignee />} />
        </Routes>
      </Box>
    </Router>
  );
};

export default App;
```
<img width="1000" alt="Screenshot 2024-01-14 at 5 53 56 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/9b0ac429-7f2d-40c5-822c-6773adc92027">

### TaskList.js
Displays a list of tasks.
```bash
// TaskList.js
import React, { useEffect, useState } from 'react';
import axios from 'axios';
import { Table, TableBody, TableCell, TableHead, TableRow, Paper, Button, Snackbar, Alert } from '@mui/material';
import DeleteIcon from '@mui/icons-material/Delete';

const TaskList = () => {
  const [tasks, setTasks] = useState([]);
  const [openSnackbar, setOpenSnackbar] = useState(false);
  const [snackbarMessage, setSnackbarMessage] = useState('');

  useEffect(() => {
    axios.get('http://localhost:8080/tasks')
      .then(response => setTasks(response.data))
      .catch(error => console.error('Error fetching tasks:', error));
  }, []);

  const handleDelete = (id) => {
    axios.delete(`http://localhost:8080/tasks/${id}`)
      .then(() => {
        setTasks(prev => prev.filter(task => task.id !== id));
        setSnackbarMessage('Task deleted successfully');
        setOpenSnackbar(true);
      })
      .catch(error => {
        console.error('Error deleting task:', error);
        setSnackbarMessage('Failed to delete the task');
        setOpenSnackbar(true);
      });
  };

  const handleCloseSnackbar = (event, reason) => {
    if (reason === 'clickaway') {
      return;
    }
    setOpenSnackbar(false);
  };

  return (
    <Paper style={{ marginTop: 20 }}>
      <Table>
        <TableHead>
          <TableRow>
            <TableCell>ID</TableCell>
            <TableCell>Name</TableCell>
            <TableCell>Assignee</TableCell>
            <TableCell>Project</TableCell>
            <TableCell>Start Time</TableCell>
            <TableCell>jaiprakashProperty</TableCell>
            <TableCell>Action</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {tasks.map((task) => (
            <TableRow key={task.id}>
              <TableCell>{task.id}</TableCell>
              <TableCell>{task.name}</TableCell>
              <TableCell>{task.assignee}</TableCell>
              <TableCell>{task.project}</TableCell>
              <TableCell>{task.startTime}</TableCell>
              <TableCell>{task.jaiprakashProperty}</TableCell>
              <TableCell>
                <Button
                  variant="contained"
                  color="secondary"
                  onClick={() => handleDelete(task.id)}
                  startIcon={<DeleteIcon />}
                >
                  Delete
                </Button>
              </TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
      <Snackbar
        open={openSnackbar}
        autoHideDuration={6000}
        onClose={handleCloseSnackbar}
        anchorOrigin={{ vertical: 'bottom', horizontal: 'left' }}
      >
        <Alert onClose={handleCloseSnackbar} severity="success" sx={{ width: '100%' }}>
          {snackbarMessage}
        </Alert>
      </Snackbar>
    </Paper>
  );
};

export default TaskList;
```
<img width="1000" alt="Screenshot 2024-01-14 at 5 55 11 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/621c21b2-9278-40ca-b026-b74d3a3d64c6">

### TaskForm.js
Allows users to create new tasks.
```bash
// TaskForm.js
import React, { useState } from 'react';
import axios from 'axios';
import { TextField, Button, Grid, Paper } from '@mui/material';

const TaskForm = () => {
  const [task, setTask] = useState({
    name: '',
    id: '',
    assignee: '',
    project: '',
    startTime: ''
  });

  const handleSubmit = (e) => {
    e.preventDefault();
    axios.put('http://localhost:8080/tasks', task)
      .then(() => {
        alert('Task created successfully');
        // Reset the form fields after successful submission
        setTask({ name: '', id: '', assignee: '', project: '', startTime: '' });
      })
      .catch(error => {
        console.error('Error creating task:', error);
        alert('Error creating task');
      });
  };

  const handleChange = (e) => {
    setTask({ ...task, [e.target.name]: e.target.value });
  };

  return (
    <Paper style={{ padding: 20, marginTop: 20 }}>
      <form onSubmit={handleSubmit}>
        <Grid container spacing={2}>
          <Grid item xs={12}>
            <TextField
              fullWidth
              label="ID"
              name="id"
              value={task.id}
              onChange={handleChange}
            />
          </Grid>
          <Grid item xs={12}>
            <TextField
              fullWidth
              label="Name"
              name="name"
              value={task.name}
              onChange={handleChange}
            />
          </Grid>
          <Grid item xs={12}>
            <TextField
              fullWidth
              label="Assignee"
              name="assignee"
              value={task.assignee}
              onChange={handleChange}
            />
          </Grid>
          <Grid item xs={12}>
            <TextField
              fullWidth
              label="Project"
              name="project"
              value={task.project}
              onChange={handleChange}
            />
          </Grid>
          <Grid item xs={12}>
            <TextField
              fullWidth
              label="Start Time"
              name="startTime"
              value={task.startTime}
              onChange={handleChange}
              type="datetime-local"
              InputLabelProps={{
                shrink: true,
              }}
            />
          </Grid>
          <Grid item xs={12}>
            <Button variant="contained" color="primary" type="submit">
              Create Task
            </Button>
          </Grid>
        </Grid>
      </form>
    </Paper>
  );
};

export default TaskForm;
```
<img width="1000" alt="Screenshot 2024-01-14 at 6 26 31 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/7941d7dc-d925-444b-aea0-4962e88b60a3">

### SearchByName Component (SearchByName.js)
Enables searching for tasks based on their name.
```bash
// SearchByName.js
import React, { useState } from 'react';
import axios from 'axios';
import { TextField, Button, Grid, Paper, Table, TableBody, TableCell, TableHead, TableRow } from '@mui/material';

const SearchByName = () => {
  const [name, setName] = useState('');
  const [foundTasks, setFoundTasks] = useState([]);
  const [searched, setSearched] = useState(false);

  const handleSearch = async () => {
    try {
      const response = await axios.get(`http://localhost:8080/tasks/search/byName?name=${name}`);
      setFoundTasks(response.data);
      setSearched(true);
    } catch (error) {
      console.error('Error searching tasks by name:', error);
      setFoundTasks([]);
      setSearched(true);
    }
  };

  return (
    <Paper style={{ padding: 20, marginTop: 20 }}>
      <Grid container spacing={2} alignItems="center" justifyContent="center">
        <Grid item>
          <TextField
            label="Task Name"
            value={name}
            onChange={(e) => setName(e.target.value)}
          />
        </Grid>
        <Grid item>
          <Button variant="contained" color="primary" onClick={handleSearch}>
            Search
          </Button>
        </Grid>
      </Grid>
      {searched && (
        <Table style={{ marginTop: 20 }}>
          <TableHead>
            <TableRow>
              <TableCell>ID</TableCell>
              <TableCell>Name</TableCell>
              <TableCell>Assignee</TableCell>
              <TableCell>Project</TableCell>
              <TableCell>Start Time</TableCell>
              <TableCell>Jaiprakash Property</TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {foundTasks.map((task) => (
              <TableRow key={task.id}>
                <TableCell>{task.id}</TableCell>
                <TableCell>{task.name}</TableCell>
                <TableCell>{task.assignee}</TableCell>
                <TableCell>{task.project}</TableCell>
                <TableCell>{task.startTime}</TableCell>
                <TableCell>{task.jaiprakashProperty}</TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      )}
    </Paper>
  );
};

export default SearchByName;
```
<img width="1000" alt="Screenshot 2024-01-14 at 6 28 43 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/cfc163ff-af6c-4687-88f4-8e0e66cecb1c">

### SearchByAssignee Component (SearchByAssignee.js)
Facilitates searching for tasks by the assignee.
```bash
// SearchByAssignee.js
import React, { useState } from 'react';
import axios from 'axios';
import { TextField, Button, Grid, Paper, Table, TableBody, TableCell, TableHead, TableRow } from '@mui/material';

const SearchByAssignee = () => {
  const [assignee, setAssignee] = useState('');
  const [foundTasks, setFoundTasks] = useState([]);
  const [searched, setSearched] = useState(false);

  const handleSearch = async () => {
    try {
      const response = await axios.get(`http://localhost:8080/tasks/search/byAssignee?assignee=${assignee}`);
      setFoundTasks(response.data);
      setSearched(true);
    } catch (error) {
      console.error('Error searching tasks by assignee:', error);
      setFoundTasks([]);
      setSearched(true);
    }
  };

  return (
    <Paper style={{ padding: 20, marginTop: 20 }}>
      <Grid container spacing={2} alignItems="center" justifyContent="center">
        <Grid item>
          <TextField
            label="Assignee"
            value={assignee}
            onChange={(e) => setAssignee(e.target.value)}
          />
        </Grid>
        <Grid item>
          <Button variant="contained" color="primary" onClick={handleSearch}>
            Search
          </Button>
        </Grid>
      </Grid>
      {searched && (
        <Table style={{ marginTop: 20 }}>
          <TableHead>
            <TableRow>
              <TableCell>ID</TableCell>
              <TableCell>Name</TableCell>
              <TableCell>Assignee</TableCell>
              <TableCell>Project</TableCell>
              <TableCell>Start Time</TableCell>
              <TableCell>Jaiprakash Property</TableCell>
            </TableRow>
          </TableHead>
          <TableBody>
            {foundTasks.map((task) => (
              <TableRow key={task.id}>
                <TableCell>{task.id}</TableCell>
                <TableCell>{task.name}</TableCell>
                <TableCell>{task.assignee}</TableCell>
                <TableCell>{task.project}</TableCell>
                <TableCell>{task.startTime}</TableCell>
                <TableCell>{task.jaiprakashProperty}</TableCell>
              </TableRow>
            ))}
          </TableBody>
        </Table>
      )}
    </Paper>
  );
};

export default SearchByAssignee;
```
<img width="1000" alt="Screenshot 2024-01-14 at 6 29 34 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/f6e750f5-68e7-420e-87db-136d8dc7cdd2">

### HomePage Component (HomePage.js)
Displays the home page with a brief description of the application.
```bash
// HomePage.js
import React from 'react';
import { Paper, Typography, Box } from '@mui/material';

const HomePage = () => {
  return (
    <Paper style={{ padding: 20, marginTop: 20 }}>
      <Box textAlign="center">
        <Typography variant="h4">Welcome to the Kaiburr Task Management System</Typography>
        <Typography variant="subtitle1" style={{ marginTop: 20 }}>
          This application provides an interface to manage and track tasks efficiently.
          Users can create, view, search, and delete tasks, as well as assign tasks to team members.
          It's designed to streamline the process of task management for small to medium-sized teams.
        </Typography>
      </Box>
    </Paper>
  );
};

export default HomePage;
```
<img width="1000" alt="Screenshot 2024-01-14 at 6 46 13 AM" src="https://github.com/reddyjai30/KAIBURR-Task3_WEB-UI-Forms_JaiPrakash/assets/47852931/abe73053-58b3-42b4-8387-7bef433860d3">

# Conclusion
The development process of this Task Management Application served as an extensive exercise in full-stack web development, encapsulating both front-end and back-end integrations. It illustrated key concepts, best practices, and the utility of popular tools and libraries in building modern web applications. This application serves as a testament to the power of combining React's dynamic capabilities with Material-UI's design efficiency, all while maintaining a clean and user-friendly interface.

