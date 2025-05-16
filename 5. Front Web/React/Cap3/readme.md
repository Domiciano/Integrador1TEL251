Puede generar componentes de la siguiente forma

```jsx
import React from 'react';
import {
  Card,
  CardHeader,
  CardContent,
  CardActions,
  Avatar,
  Typography,
  Button
} from '@mui/material';

export default function UserCard({ user }) {
  return (
    <Card sx={{ maxWidth: 345, m: 2 }}>
      <CardHeader
        avatar={<Avatar>{user.name[0]}</Avatar>}
        title={user.name}
        subheader={user.email}
      />
      <CardContent>
        <Typography variant="body2" color="text.secondary">
          {user.bio}
        </Typography>
      </CardContent>
      <CardActions>
        <Button size="small">Ver perfil</Button>
        <Button size="small" color="secondary">Eliminar</Button>
      </CardActions>
    </Card>
  );
}
```


Para usar ese componente recien creado

```jsx
import React from 'react';
import UserCard from './UserCard';

const user = {
  name: 'Juan Pérez',
  email: 'juan@example.com',
  bio: 'Desarrollador frontend con experiencia en React y Vue.'
};

function App() {
  return <UserCard user={user} />;
}

export default App;
```

```jsx
```
