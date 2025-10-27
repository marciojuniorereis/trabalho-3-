# trabalho-3-
trabalo 3
projeto/
│
├─ index.html
├─ /css
│   └─ style.css
├─ /js
│   ├─ main.js
│   ├─ router.js
│   ├─ views.js
│   └─ contacts.js
└─ /img
<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <title>Agenda de Contatos</title>
    <link rel="stylesheet" href="css/style.css">
</head>
<body>
    <header>
        <h1>Agenda de Contatos</h1>
        <nav>
            <button data-route="home">Home</button>
            <button data-route="contatos">Contatos</button>
        </nav>
    </header>

    <main id="app">
        <!-- Conteúdo da SPA será carregado aqui -->
    </main>

    <script src="js/views.js"></script>
    <script src="js/contacts.js"></script>
    <script src="js/router.js"></script>
    <script src="js/main.js"></script>
</body>
</html>
body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 0;
}

header {
    background-color: #f0f0f0;
    padding: 10px 20px;
}

header h1 {
    margin: 0 0 10px 0;
}

nav button {
    margin-right: 10px;
    padding: 5px 10px;
    cursor: pointer;
}

main {
    padding: 20px;
}

form input, form button {
    display: block;
    margin: 10px 0;
    padding: 5px;
}
document.addEventListener("DOMContentLoaded", () => {
    router.loadRoute("home");

    document.querySelectorAll("nav button").forEach(btn => {
        btn.addEventListener("click", () => {
            const route = btn.getAttribute("data-route");
            router.loadRoute(route);
        });
    });
});
const router = {
    routes: {
        home: views.home,
        contatos: views.contatos
    },
    loadRoute(route) {
        const app = document.getElementById("app");
        if (this.routes[route]) {
            app.innerHTML = this.routes[route]();
            if (route === "contatos") {
                contacts.loadContacts();
                const form = document.getElementById("contactForm");
                form.addEventListener("submit", contacts.addContact);
            }
        }
    }
};
const views = {
    home: () => {
        return `
            <h2>Bem-vindo à Agenda de Contatos</h2>
            <p>Use o menu acima para cadastrar ou listar seus contatos.</p>
        `;
    },
    contatos: () => {
        return `
            <h2>Contatos</h2>
            <div id="contactList"></div>
            <h3>Novo Contato</h3>
            <form id="contactForm">
                <input type="text" id="name" placeholder="Nome" required>
                <input type="email" id="email" placeholder="Email" required>
                <input type="text" id="phone" placeholder="Telefone">
                <button type="submit">Adicionar</button>
            </form>
        `;
    }
};
const contacts = {
    loadContacts() {
        const listDiv = document.getElementById("contactList");
        const saved = JSON.parse(localStorage.getItem("contacts")) || [];
        if (saved.length === 0) {
            listDiv.innerHTML = "<p>Nenhum contato cadastrado.</p>";
            return;
        }

        let html = "<ul>";
        saved.forEach((c, index) => {
            html += `<li>${c.name} - ${c.email} ${c.phone ? "- " + c.phone : ""}</li>`;
        });
        html += "</ul>";
        listDiv.innerHTML = html;
    },

    addContact(event) {
        event.preventDefault();
        const name = document.getElementById("name").value.trim();
        const email = document.getElementById("email").value.trim();
        const phone = document.getElementById("phone").value.trim();

        if (!name || !email) {
            alert("Nome e Email são obrigatórios!");
            return;
        }

        const saved = JSON.parse(localStorage.getItem("contacts")) || [];
        saved.push({ name, email, phone });
        localStorage.setItem("contacts", JSON.stringify(saved));

        document.getElementById("contactForm").reset();
        contacts.loadContacts();
    }
};
