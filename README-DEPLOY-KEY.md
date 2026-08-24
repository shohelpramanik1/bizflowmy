# GitHub Deploy Key

A dedicated **ed25519** SSH key pair is included in this folder for CI/CD and server deploys.

| File | Purpose |
|------|---------|
| `deploy-key.pub`  | **Public key** — add this as a *Deploy Key* in your GitHub repository (read-only or read+write) or as an SSH key on your deployment server. |
| `deploy-key`      | **Private key** — keep this secret. Add it to your CI/CD secrets (e.g. `SSH_PRIVATE_KEY` in GitHub Actions, GitLab CI, Vercel, Netlify, Render, etc.). **Never commit it to a public repo.** |

## Fingerprint

```
256 SHA256:GtEHqjoxeWZJz+t+GMpXB656ZsUYd+T27AUVAa2JeDc bizflow-my-deploy-key-2026 (ED25519)
```

## How to add the key to a GitHub repository

1. Open your repo on GitHub → **Settings** → **Deploy keys**.
2. Click **Add deploy key**.
3. Paste the contents of `deploy-key.pub` into the *Key* field.
4. Give it a title (e.g. `deploy@bizflow-my-2026`).
5. Tick **Allow write access** only if your CI needs to push (e.g. release tags).
6. Click **Add key**.

## How to use the key in CI

Add the private key as a repository secret named `SSH_PRIVATE_KEY`. In your workflow:

```yaml
- name: Install SSH key
  uses: shimataro/ssh-key-action@v2
  with:
    key: ${{ secrets.SSH_PRIVATE_KEY }}
    known_hosts: unnecessary
```

Or with plain bash:

```bash
mkdir -p ~/.ssh
echo "$SSH_PRIVATE_KEY" > ~/.ssh/id_ed25519
chmod 600 ~/.ssh/id_ed25519
ssh-keyscan github.com >> ~/.ssh/known_hosts
```

## Regenerating

If you need to rotate the key:

```bash
ssh-keygen -t ed25519 -C "bizflow-my-deploy-key-$(date +%Y)" -f .github/deploy-key -N "" -q
```

⚠️ If your repository is public, remove the private key (`deploy-key`) from this folder after adding it to your CI secret store — commit **only** the `.pub` file. In this template the private key is present as a convenience for local/demo setup; generate a fresh pair before using it in production.
