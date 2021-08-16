# PlayerMovementProblem
using UnityEngine;

public class PlayerMovement : MonoBehaviour
{
    //Variables
    float moveForward;
    float moveSide;
    float moveUp;

    bool isGrounded;

    public float walkSpeed = 1f;
    public float sprintSpeed;
    float currentSpeed;
    public float jumpSpeed = 5f;
    Rigidbody rb;

    void Start()
    {
        //whatever this shit is, it's important
        rb.GetComponent<Rigidbody>();
    }

    void Update()
    {
        //sprint
        if (Input.GetKey(KeyCode.LeftShift))
        {
            if (currentSpeed != sprintSpeed)
                currentSpeed = sprintSpeed;
        }
        else
            currentSpeed = walkSpeed;

        //WASD
        moveForward = Input.GetAxis("Vertical") * currentSpeed;
        moveSide = Input.GetAxis("Horizontal") * currentSpeed;
        moveUp = Input.GetAxis("Jump") * jumpSpeed;
    }
    private void FixedUpdate() //HAVING TROUBLE HERE!!!!
    {
        rb.velocity = (transform.forward * moveForward) + (transform.right * moveSide) + (transform.up * rb.velocity.y);

        //Makes character jump
        if (isGrounded && moveUp != 0)
        {
            rb.AddForce(transform.up * moveUp, ForceMode.VelocityChange);
            isGrounded = false;
        }
    }
    private void OnCollisionEnter(Collision collision)
    {
        //Collision with ground
        if (collision.gameObject.tag == "Ground")
            isGrounded = true;
    }
}
