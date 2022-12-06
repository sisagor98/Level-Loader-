# Level-Loader-
using DG.Tweening;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.SceneManagement;
using UnityEngine.UI;


public class LoadingBar : MonoBehaviour
{
    [SerializeField] Slider slider;
    AsyncOperation asyncLoad;

    private void Start()
    {
        slider.value = 0;
        StartCoroutine(LoadAsyncScene());
    }

    IEnumerator LoadAsyncScene()
    {
        asyncLoad = SceneManager.LoadSceneAsync(LevelLoader.Instance.Actual_Level_Index());
        asyncLoad.allowSceneActivation = false;
        float startValue = 0;
        // if want to normal scene loader ... remove or commet out DoTween 
        
        DOTween.To(() => startValue, x => startValue = x, 1.0f, 4.0f).OnUpdate(delegate
        {
            slider.value = startValue;
        }).OnComplete(delegate
        {
            asyncLoad.allowSceneActivation = true;
        });

        yield return null;
        ///////if asyncLoad load  remove ...// 
        //while (!asyncLoad.isDone)
        //{
        //    slider.value = asyncLoad.progress;
        //    if (asyncLoad.progress >= 0.9f)
        //    {
        //        asyncLoad.allowSceneActivation = true;
        //    }
        //    yield return null;
        //}
    }
}
