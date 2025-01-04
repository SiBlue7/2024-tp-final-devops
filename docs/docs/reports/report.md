> Place the content of your report here (you can have multiple files with images and all)
>
> See the docusaurus documentation if needed

# Méthode de travail

## Méthode de travail en temps normal
1. En premier on a décicé de plusieurs tâches à acomplir.
2. Ensuite la méthode choisit sur github est de 1 branche pour 1 fonctionnalités.
3. A chaque fonctionnalités fini sur 1 branche, 1 pull request est faite.
4. Suite à chaque pull request, l'autre développeur doit repasser sur le code pour donner des conseils ou des remarques.
5. Enfin, après la vérification, la pull request est merge sur la main.

## Méthode de travail lors d'un bug bloquant ou majeur nécessitant un hot fix
1. Lors d'un hot fix, le développeur en charge de le régler, va regarder le problème :
    - Si le problème est facile à régler alors le développeur va coder la solution sur la main
    - Autre cas, si le problème nécéssite un peu plus de temps alors une backup sans la/les fonctionnalités bloquantes sera fait

# Contribuer au projets
1. Deux choix pour commencer :
    - Vous pouvez consulter la listes des fonctionnalités que l'on ajoutera dans le futur mais dont nous n'avons pas commencé le développement.
    - Ou alors vous penser que une fonctionnalité est manquante, alors vous pouvez commencer à la mettre en place.
2. Pour pouvoir commencer à développer nous vous conseillons de faire un fork de notre github.
3. Maintenant le fork créer, vous pouvez développer votre fonctionnalité. Une fois celle-ci finis vous pouvez créer une pull request.
4. Dès que possible votre pull request sera analysé, et si il n'y a aucun problème de notre côté alors elle sera accepté. Si on rencontre un problème un retour vous sera fait.

# Docker
Le projet contient 3 dockerfile, 1 pour la docs, 1 pour vote-api et 1 pour web-client.
Chacun de ces dockerfile respectent les consignes suivantes :

- L'application ne doit PAS être exécutée en tant que root dans le conteneur.
- L'image latest de votre conteneur doit être la dernière version de l'application.
- Utilisez des builds multi-étapes pour garder l'image finale aussi petite que possible.

# Docker compose

# CI
La CI contient actuellement, le check automatique sur toutes les branches du lint, des tests, du build et du format de l'api. Ainsi que le check du build de la docs. Et pour le web-client, le check du lint, du build et du format.

Pour que la CI soit complète, il manque pour moi le check des tests pour le web-client, il manque aussi le push des images docker pour l'api et le web-client.

# CD

Pour la CD qui ne se lancerai que lorsqu'un push sur la main est fait, les solutions qu'on voulait mettre en place étaient :
    - Pour la mise en ligne de la docs, utiliser github pages car Docusaurus permet de le faire plus facilement.
    - Pour la mise en ligne de l'api et du web-client, on serait parti sur l'utilisation de render, en se servant de base le code suivant.
    ```
    deploy-on-render:
    needs: build-on-docker-and-push-to-dockerhub
    runs-on: ubuntu-latest
    steps:
      - name: deploy on render.com
        env:
          RENDER_API_KEY: ${{ secrets.RENDER_API_KEY }}
          RENDER_DEPLOY_HOOK: ${{ secrets.RENDER_DEPLOY_HOOK }}
        run: |
            curl -X POST \
            -H "Authorization: Bearer $RENDER_API_KEY" \
            -H "Content-Type: application/json" \
            $RENDER_DEPLOY_HOOK
    ```

# Deployment
Le deploiement de l'application dans notre cas à logiquement besoin de deux environnements, 1 préproduction et 1 autre production. Pour que ca fonctionne, on peux mettre en place :
    - Dès qu'un commit est fait sur branche autre que la main, le code part sur la préproduction
    - Dès qu'une pull request est envoyé et accepté sur la main alors le code est envoyé en production.

# Blocage

1. Problème rencontré lors de la mise en place en CI, de l'automatisation des tests pour le web-client.
```
web-tests:
        runs-on: ubuntu-latest
        needs: web-build
        steps:
        - name: Checkout code
          uses: actions/checkout@v4
          
        - name: Setup Node
          uses: actions/setup-node@v4
          with:
            node-version: 22

        - run: |
            cd web-client
            yarn install --no-lockfile
            yarn playwright install
            yarn test
            npx playwright install --with-deps
            yarn e2e

```
2. Problème pour push les images sur docker hub