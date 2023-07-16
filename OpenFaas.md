# Open Faas

To login into the gateway.

```bash
faas-cli login --gateway https://gateway-openfaas.aminmag.ir/ --username admin --password 3124gZi
```

See all of the available templates.

```bash
faas-cli template store list
```

Choose between templates and pull one.

```bash
faas-cli template store pull node10-express
```

Create new template

```bash
faas-cli new --lang python hello-python
```

After you change the handler or function you should build and push your FaaS application.

```bash
sudo faas-cli build -f ./hello-world.yml
```

To deploy your function.

```bash
faas-cli deploy -f ride-recommend.yml
```