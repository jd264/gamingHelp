#pragma strict

var moveSpeed : float = 10.0;
var jSpeed : float = 10.0;
var grav : float = 20.0;
var dashSpeed : float = 20.0;
var dashCooldown : float = 3.0;

private var moveDir : Vector3 = Vector3.zero;

function Start () {

}

function Update () 
{

    var cC : CharacterController = GetComponent.<CharacterController>();

    if(cC.isGrounded)
    {
        moveDir = Vector3(Input.GetAxis("Horizontal"), 0, 0);
        moveDir = transform.TransformDirection(moveDir);
        moveDir *= moveSpeed;

        if(Input.GetButton("Jump"))
        {
            moveDir.y = jSpeed;
        }
        
        if(Input.GetKey(KeyCode.R) && dashCooldown == 3.0)
        {
            moveDir *= 50.0;
            dashCooldown = 0.0;
        }

    }
    else
    {
        moveSpeed = 5.0;
        
    }

    if (dashCooldown >= 0.25)
    {
        moveSpeed = 10.0;
    }
    dashCooldown += 1.0 * Time.deltaTime;
    if (dashCooldown >= 3.0)
    {
        dashCooldown = 3.0;
    }

    Debug.Log(dashCooldown);
    Debug.Log(moveSpeed);

    moveDir.y -= grav * Time.deltaTime;

    cC.Move(moveDir * Time.deltaTime);
}
