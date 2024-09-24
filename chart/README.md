# Documenso Chart (unofficial)

## Prerequisites
- Ensure you have Helm installed. If not, follow the [Helm installation guide](https://helm.sh/docs/intro/install/).
- MySQL / PostgreSQL Database
- (Optional) Oauth2 such as Google, Github etc.


## Step 1: Install the Helm Chart
1. Add the chart repository (if using a remote repository):
    ```sh
    helm repo add my-repo 
    ```
2. Update the repository to get the latest charts:
    ```sh
    helm repo update
    ```
3. Install the chart:
    ```sh
    helm install my-release /path/to/documenso-chart-<version>.tgz
    ```
    Replace `<version>` with the actual version number of the packaged chart.

## Step 5: Verify the Installation
1. Check the status of the release:
    ```sh
    helm status my-release
    ```
2. Verify the deployed resources in your Kubernetes cluster:
    ```sh
    kubectl get all -l release=my-release
    ```

## Step 6: Upgrade the Helm Chart (if needed)
1. Make further modifications to the chart.
2. Upgrade the release with the updated chart:
    ```sh
    helm upgrade my-release /path/to/documenso-chart-<version>.tgz
    ```

## Step 7: Uninstall the Helm Chart (if needed)
1. Uninstall the release:
    ```sh
    helm uninstall my-release
    ```

## Additional Resources
- [Helm Documentation](https://helm.sh/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Documenso Official Page](https://documenso.com)
