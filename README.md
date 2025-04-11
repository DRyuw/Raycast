# Raycast

## Vídeo da Cena:

https://github.com/user-attachments/assets/4f32cfdd-7006-40c0-8027-f9f5be82c0a9

----
GameObjects usados: 4 Retângulos para criar uma sala e um solo, e uma esfera para poder ser o objeto usado com o Raycast.
---

<h2>Como funciona</h2>: Movendo a câmera você é capaz de lançar um Raycast na direção que você está olhando, se atingir a esfera ela se destroi e cria outras duas em um raio de 0 a 5f no ângulo X, assim se repetindo a cada esfera atingida.
Resumidamente ele destrói o prefab original e cria uma duas copias de cores diferentes em um raio de 0f a 5f.

Código para tanto poder mover a câmera e poder utilizar o mouse para poder destruir e criar esferas:

```

using UnityEngine;

public class RaycastSpawner : MonoBehaviour
{
    public GameObject itemPrefab;
    public Camera mainCamera; // arraste a câmera no Inspector

    void Update()
    {
        if (Input.GetButtonDown("Fire1"))
        {
            Ray ray = mainCamera.ScreenPointToRay(Input.mousePosition);
            if (Physics.Raycast(ray, out RaycastHit hit))
            {
                if (hit.collider.gameObject.layer == LayerMask.NameToLayer("Item"))
                {
                    Vector3 originalPos = hit.collider.transform.position;
                    Quaternion rotation = hit.collider.transform.rotation;

                    // Destroi o objeto atingido
                    Destroy(hit.collider.gameObject);

                    // Instancia 2 clones com cor e posição aleatórias
                    for (int i = 0; i < 2; i++)
                    {
                        Vector3 randomOffset = Random.insideUnitSphere * 5f;
                        randomOffset.y = 0f; // mantém no mesmo nível

                        GameObject clone = Instantiate(itemPrefab, originalPos + randomOffset, rotation);

                        // Tenta mudar a cor, assumindo que há um Renderer com um material
                        Renderer rend = clone.GetComponent<Renderer>();
                        if (rend != null)
                        {
                            rend.material.color = Random.ColorHSV(); // cor aleatória
                        }
                    }
                }
            }
        }
    }
} 

```

