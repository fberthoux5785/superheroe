Claro! Vou ajustar o código para incluir o seu **Access Token** fornecido. Aqui está a versão corrigida com seu token.

### **Código Completo da Aplicação em Next.js com seu Token**

**1. `pages/index.js`** (Página principal)

Este arquivo vai fazer a requisição à API usando seu token e renderizar os cards dos heróis.

```javascript
import { useEffect, useState } from "react";
import axios from "axios";
import HeroCard from "../components/HeroCard";
import styles from "../styles/Home.module.css";

const ACCESS_TOKEN = "136c3a38c87e0b1984b69f8896b03740"; 
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

Este componente exibe as informações dos heróis.

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

### Como rodar o projeto:

1. **Inicie o servidor de desenvolvimento** executando o seguinte comando:

```bash
npm run dev
```

2. Acesse a aplicação em [http://localhost:3000](http://localhost:3000) para ver os resultados.
