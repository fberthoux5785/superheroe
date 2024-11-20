# superheroe
Claro! Vou ajustar o texto para uma explicação mais fluida e revisar o código para garantir que está correto e funcional.

---

### Estrutura do Projeto em Next.js

Vamos reconstruir a aplicação utilizando **Next.js** e exibir informações de heróis em cards, com base em dados fornecidos pela **API de Super-Heróis**.

### Passos para Criar o Projeto:

1. **Criação do Projeto**:
   
   Abra seu terminal e crie um novo projeto Next.js. Execute os seguintes comandos:

   ```bash
   npx create-next-app@13 superhero-app
   cd superhero-app
   npm install axios
   ```

2. **Obtenção do Token de Acesso**:
   
   Vá até [SuperheroAPI](https://superheroapi.com) e gere um **Access Token** para usar na aplicação.

3. **Estrutura do Projeto**:

   O projeto terá a seguinte estrutura:

   ```
   /superhero-app
     /components
       HeroCard.js
     /pages
       index.js
     /styles
       globals.css
       Home.module.css
   ```

### Código Completo da Aplicação:

**1. `pages/index.js`** (Página Principal)

Aqui, fazemos a requisição à API para obter os dados dos heróis e renderizamos os cards.

```javascript
import { useEffect, useState } from "react";
import axios from "axios";
import HeroCard from "../components/HeroCard";
import styles from "../styles/Home.module.css";

const ACCESS_TOKEN = "seu_token_aqui";
const BASE_URL = "https://superheroapi.com/api.php/";

export default function Home() {
  const [heroes, setHeroes] = useState([]);

  useEffect(() => {
    const fetchHeroes = async () => {
      const heroIds = [200, 465];
      const heroPromises = heroIds.map((id) =>
        axios.get(`${BASE_URL}${ACCESS_TOKEN}/${id}`)
      );
      try {
        const responses = await Promise.all(heroPromises);
        const heroesData = responses.map((response) => response.data);
        setHeroes(heroesData);
      } catch (error) {
        console.error("Erro ao obter heróis:", error);
      }
    };

    fetchHeroes();
  }, []);

  return (
    <div className={styles.container}>
      <div id="heroes" className={styles.heroGrid}>
        {heroes.map((hero) => (
          <HeroCard key={hero.id} hero={hero} />
        ))}
      </div>
    </div>
  );
}
```

**2. `components/HeroCard.js`** (Componente para exibir os cards)

Este componente exibe as informações de cada herói.

```javascript
import styles from "../styles/Home.module.css";

const HeroCard = ({ hero }) => {
  const { name, image, powerstats } = hero;
  const { intelligence, strength } = powerstats;

  return (
    <article className={styles.heroCard}>
      <img src={image.url} alt={name} />
      <h1>{name}</h1>
      <p>
        Intelligence: <span style={{ width: `${intelligence}%` }}></span>
      </p>
      <p>
        Strength: <span style={{ width: `${strength}%` }}></span>
      </p>
    </article>
  );
};

export default HeroCard;
```

**3. Estilos CSS**

Aqui estão os estilos necessários para a aplicação.

**`globals.css`**:

```css
html, body {
  background-color: #f0f0f0;
  font-family: monospace;
  height: 100%;
  margin: 0;
}
```

**`Home.module.css`**:

```css
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}

.heroGrid {
  display: flex;
  flex-flow: row wrap;
  width: 100%;
  padding: 20px;
  justify-content: center;
}

.heroCard {
  height: 720px;
  width: 300px;
  background-color: #fff;
  border-radius: 10px;
  margin: 10px;
  padding: 10px;
  text-align: center;
}

.heroCard img {
  border-radius: 10px 10px 0 0;
  width: 100%;
  max-height: 400px;
}

.heroCard h1 {
  font-size: 20px;
}

.heroCard p {
  padding: 0 10px;
  width: calc(100% - 20px);
}

.heroCard p span {
  background-color: red;
  height: 10px;
  display: block;
  margin-top: 5px;
  border-radius: 5px;
}
```

### Executando o Projeto:

Para rodar o projeto, execute o seguinte comando no terminal:

```bash
npm run dev
```
