using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
public class PlayerHP : MonoBehaviour
{
    private float HP; //Máu hiện tại
    private float lerpTimer;
    public float maxHP = 100f; //Máu tối đa
    public float chipSpeed = 2f;

    public Image FronHP; //Thanh máu
    public Image BackHP; //Dư ảnh sau khi mất máu
    // Start is called before the first frame update
    void Start()
    {
        HP = maxHP; //Sau khi vào game máu = máu tối đa
    }

    // Update is called once per frame
    void Update()
    {
        HP = Mathf.Clamp(HP, 0, maxHP);
        UpdateHPUI();
        if (Input.GetKeyDown(KeyCode.A))
        {
            TakeDamage(Random.Range(5, 10));
        }
        if (Input.GetKeyDown(KeyCode.S))
        {
            RestoreHP(10);
        }
    }
    public void UpdateHPUI()
    {
        Debug.Log(HP);
        float fillF = FronHP.fillAmount;
        float fillB = BackHP.fillAmount;
        float hFraction = HP / maxHP;
        if (fillB > hFraction)
        {
            FronHP.fillAmount = hFraction;
            BackHP.color = Color.red;
            lerpTimer += Time.deltaTime;
            float percentComplete = lerpTimer / chipSpeed;
            BackHP.fillAmount = Mathf.Lerp(fillB, hFraction, percentComplete);
        }
        if(fillF < hFraction)
        {
            BackHP.color = Color.green;
            BackHP.fillAmount = hFraction;
            lerpTimer += Time.deltaTime;
            float percentComplete = lerpTimer / chipSpeed;
            percentComplete = percentComplete * percentComplete;
            FronHP.fillAmount = Mathf.Lerp(fillF, BackHP.fillAmount, percentComplete);
        }
    }
    public void TakeDamage(float damage)
    {
        HP -= damage;
        lerpTimer = 0f;
    }
    public void RestoreHP(float HPamount)
    {
        HP += HPamount;
        lerpTimer = 0f;
    }
}
