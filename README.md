prac7
Rangehecker
prac7_1.cs
using UnityEngine;

public class prac7_1 : MonoBehaviour
{
    [SerializeField] Transform target;
    [SerializeField] float range = 5;

    private bool targetWasInRange = false;

    void Update()
    {
        var distance = (target.position - transform.position).magnitude;

        if(distance <= range && targetWasInRange == false){
            Debug.Log("Target entered range");
            targetWasInRange = true;
        }
        else if (distance > range && targetWasInRange == true){
            Debug.Log("Target exited range");
            targetWasInRange = false;
        }
    }
}
prac7_2.cs
using UnityEngine;

public class prac7_2 : MonoBehaviour
{
    [SerializeField] Transform target;

    void Update()
    {
        var directionToTarget = (target.position - transform.position).normalized;

        var dotProduct = Vector3.Dot(transform.forward, directionToTarget);

        var angle = Mathf.Acos(dotProduct);

        Debug.Log("Angle is " + angle * Mathf.Rad2Deg);
    }
}

Prac8
PLAYER.cs
using UnityEngine;

public class PLAYER : MonoBehaviour
{
 public float speed = 5f;
 Rigidbody rb;

 void Start()
 {
  rb = GetComponent<Rigidbody>();
 }

 void FixedUpdate()
 {
  float horizontal = Input.GetAxis("Horizontal");
  float vertical = Input.GetAxis("Vertical");

  Vector3 move = new Vector3(horizontal,0,vertical)*speed;

  rb.linearVelocity = new Vector3(move.x, rb.linearVelocity.y, move.z);
 }
}

ENEMY.cs
using UnityEngine;
using UnityEngine.AI;

public class ENEMY : MonoBehaviour
{
 public Transform target;
 public float speed = 3.5f;

 private NavMeshAgent agent;

 void Start()
 {
  agent = GetComponent<NavMeshAgent>();
  agent.speed = speed;
 }

 void Update()
 {
  if(target != null)
  {
   agent.SetDestination(target.position);
  }
 }
}
GOAL.cs
using UnityEngine;

public class GOAL : MonoBehaviour
{
 void OnTriggerEnter(Collider other)
 {
  if(other.CompareTag("Player"))
  {
   Debug.Log("You Win!");
   Time.timeScale = 0;
  }

  else if(other.CompareTag("Enemy"))
  {
   Debug.Log("AI Wins!");
   Time.timeScale = 0;
  }
 }
}
