const items=[...document.querySelectorAll('[data-reveal]')];
if('IntersectionObserver' in window){
  const observer=new IntersectionObserver(entries=>{entries.forEach(entry=>{if(entry.isIntersecting){entry.target.classList.add('is-visible');observer.unobserve(entry.target)}})},{threshold:.16,rootMargin:'0px 0px -6%'});
  items.forEach(el=>observer.observe(el));
}else{items.forEach(el=>el.classList.add('is-visible'))}

const saveFab=document.querySelector('.save-fab');
const saveOverlay=document.querySelector('.save-overlay');
const saveModal=document.querySelector('.save-modal');
const saveClose=document.querySelector('.save-modal__close');
const saveHelp=document.querySelector('.save-help');
const saveToast=document.querySelector('.save-toast');
let lastFocused=null;

const showSaveButton=()=>saveFab.classList.toggle('is-visible',window.scrollY>180);
showSaveButton();
window.addEventListener('scroll',showSaveButton,{passive:true});

function openSave(){
  lastFocused=document.activeElement;
  saveOverlay.hidden=false;
  requestAnimationFrame(()=>saveOverlay.classList.add('is-open'));
  document.body.classList.add('modal-open');
  saveHelp.classList.remove('is-visible');
  saveHelp.textContent='';
  setTimeout(()=>saveClose.focus(),50);
}
function closeSave(){
  saveOverlay.classList.remove('is-open');
  document.body.classList.remove('modal-open');
  setTimeout(()=>{saveOverlay.hidden=true;(lastFocused||saveFab).focus()},250);
}
function toast(message){
  saveToast.textContent=message;
  saveToast.classList.add('is-visible');
  setTimeout(()=>saveToast.classList.remove('is-visible'),2200);
}
async function copyLink(){
  try{await navigator.clipboard.writeText(location.href);toast('リンクをコピーしました。')}
  catch{const ta=document.createElement('textarea');ta.value=location.href;document.body.appendChild(ta);ta.select();document.execCommand('copy');ta.remove();toast('リンクをコピーしました。')}
}
function showHelp(html){saveHelp.innerHTML=html;saveHelp.classList.add('is-visible');saveHelp.scrollIntoView({behavior:'smooth',block:'nearest'})}

saveFab.addEventListener('click',openSave);
saveClose.addEventListener('click',closeSave);
saveOverlay.addEventListener('click',e=>{if(e.target===saveOverlay)closeSave()});
document.addEventListener('keydown',e=>{if(e.key==='Escape'&&!saveOverlay.hidden)closeSave()});
saveModal.addEventListener('keydown',e=>{
  if(e.key!=='Tab')return;
  const focusable=[...saveModal.querySelectorAll('button,[href],[tabindex]:not([tabindex="-1"])')];
  const first=focusable[0],last=focusable[focusable.length-1];
  if(e.shiftKey&&document.activeElement===first){e.preventDefault();last.focus()}
  else if(!e.shiftKey&&document.activeElement===last){e.preventDefault();first.focus()}
});

document.querySelectorAll('[data-save-action]').forEach(btn=>btn.addEventListener('click',async()=>{
  const action=btn.dataset.saveAction;
  if(action==='copy')await copyLink();
  if(action==='share'){
    if(navigator.share){try{await navigator.share({title:document.title,text:'島中星輝｜From Thought to Action.',url:location.href})}catch(err){if(err.name!=='AbortError')await copyLink()}}
    else await copyLink();
  }
  if(action==='bookmark')showHelp(/Mac|iPhone|iPad/.test(navigator.platform)?'<strong>お気に入りへの保存</strong><br>Macは <kbd>⌘</kbd> + <kbd>D</kbd>。iPhone・iPadはSafariの共有ボタンから「お気に入りに追加」を選んでください。':'<strong>お気に入りへの保存</strong><br>パソコンは <kbd>Ctrl</kbd> + <kbd>D</kbd>。スマートフォンはブラウザのメニューから「ブックマーク」または「お気に入り」を選んでください。');
  if(action==='home')showHelp(/iPhone|iPad|iPod/.test(navigator.userAgent)?'<strong>ホーム画面に追加</strong><br>Safari下部の共有ボタンを押し、「ホーム画面に追加」を選んでください。':'<strong>ホーム画面に追加</strong><br>ブラウザ右上のメニューを開き、「ホーム画面に追加」または「アプリをインストール」を選んでください。');
  if(action==='print')window.print();
}));

