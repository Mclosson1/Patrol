# Patrol
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class patrol : MonoBehaviour
{
    public float speed;
    private float waitTime;
    public float startWaitTime;

    public GameObject explosionEffect;

    public Transform moveSpot;
    public float minX;
    public float maxX;
    public float minY;
    public float maxY;

    private float timeBtwShots;
    public float startTimeBtwShots;

    public GameObject projectile;
    private Transform player;

    public GameObject[] spawnees;
    public Transform spawnPos;
    int randomInt; 
    // Start is called before the first frame update
    void Start()
    {
        waitTime = startWaitTime;
        moveSpot.position = new Vector2(Random.Range(minX, maxX), Random.Range(minY, maxY));

        player = GameObject.FindGameObjectWithTag("Player").transform;
        timeBtwShots = startTimeBtwShots;
    }

    // Update is called once per frame
    void Update()
    {
        transform.position = Vector2.MoveTowards(transform.position, moveSpot.position, speed * Time.deltaTime);
        
        if(Vector2.Distance(transform.position, moveSpot.position) < 0.2f)
        {
            if(waitTime <= 0)
            {
                moveSpot.position = new Vector2(Random.Range(minX, maxX), Random.Range(minY, maxY));
                waitTime = startWaitTime;
            }
        }
        else
        {
            waitTime -= Time.deltaTime;
        }
        if (timeBtwShots <= 0)
        {
            Instantiate(projectile, transform.position, Quaternion.identity);
            timeBtwShots = startTimeBtwShots;
        }
        else
        {
            timeBtwShots -= Time.deltaTime;
        }
    }
    void OnTriggerEnter2D(Collider2D other)
    {
        if (other.CompareTag("projectile"))
        {
            Instantiate(explosionEffect, transform.position, transform.rotation);
            score.blastscore += 500;
            Destroy(gameObject);
            SpawnRandom();
        }
        else if (other.CompareTag("Player") || other.CompareTag("Base") || other.CompareTag("obstacle"))
        {
            Instantiate(explosionEffect, transform.position, transform.rotation);
            Destroy(gameObject);

        }
    }
    void SpawnRandom()
    {
        randomInt = Random.Range(0, spawnees.Length);
        Instantiate(spawnees[randomInt], spawnPos.position, spawnPos.rotation);
    }
}
