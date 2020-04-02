<template>
  <div class="resume">
    <div class="banner">
      <div class="banner__fullname">{{ person.name.first }} {{ person.name.middle }} {{ person.name.last }}</div>
      <div class="banner__position">{{ person.position }}</div>
    </div>

    <div class="content">
      <div class="content__left">
        <div class="section">
          <div class="section-headline">
            {{ lang.about }}
          </div>

          <div class="section-content section-content--plain">
            {{ person.about }}
            <br/>
            <br/>
            {{ person.knowledge }}
          </div>
        </div>

        <div class="selection" style="margin-bottom: 20px">
          <div class="section-headline">
            {{ lang.personalInfo }}
          </div>
          <div class="section-item">
            <span>{{ lang.location }}</span>
            <span class="section-item-right">{{person.birth.location}}</span>
          </div>
          <div class="section-item" style="margin-top: 2px">
            <span>{{lang.birthDate}}</span>
            <span class="section-item-right">{{person.birth.day}}.{{person.birth.month}}.{{person.birth.year}}</span>
          </div>
        </div>

        <div class="selection">
          <div class="section-headline">
            {{ lang.skills }}
          </div>
          <div class="skill" v-for="skill in person.skills" :key="skill.name">
            <div class="right">
              <span>{{skill.name}}</span>
              <div class="progress">
                <div class="determinate" :style="'width: '+skill.level+'%;'">
                  <i class="fa fa-circle"></i>
                </div>
              </div>
            </div>
          </div>
        </div>

        <div class="section" style="margin-top: 50px;">
          <div class="section-headline">
            {{lang.languages}}
          </div>
          <div>
            <div class="section-content__item" style="margin-bottom: 2px;" v-for="language in person.languages" :key="language.name">
              <div class="section-item">
                <span>{{language.name}}</span>
                <span class="section-item-right">{{language.level}}</span>
              </div>
            </div>
          </div>
        </div>

        <div class="section">
          <div class="section-headline">
            {{ lang.contact }}
          </div>

          <div class="section-content section-content--plain">
            <a
              class="section-link"
              :href="contactLinks.email">
              <i class="section-link__icon material-icons">mail</i>{{ person.contact.email }}
            </a>

            <div class="section-link">
              <i class="section-link__icon material-icons">phone</i>{{ person.contact.phone }}
            </div>

            <a
              v-if="person.contact.telegram"
              class="section-link"
              :href="contactLinks.telegram">
              <i class="section-link__icon fa fa-paper-plane" style="-webkit-transform: scale(0.9, 0.9)"></i>{{ person.contact.telegram }}
            </a>

            <a
              v-if="person.contact.linkedin"
              class="section-link"
              :href="contactLinks.linkedin">
              <i class="section-link__icon fa fa-linkedin"></i>{{ person.contact.linkedin }}
            </a>

            <a
              v-if="person.contact.github"
              class="section-link"
              :href="contactLinks.github">
              <i class="section-link__icon fa fa-github" aria-hidden="true" style="-webkit-transform: scale(1.1, 1.1)"></i>&nbsp;{{ person.contact.github }}
            </a>

            <a
              v-if="person.contact.medium"
              class="section-link"
              :href="contactLinks.medium">
              <i class="section-link__icon fa fa-medium"></i>{{ person.contact.medium }}
            </a>
          </div>
        </div>
      </div>

      <div class="content__right">
        <div class="section">
          <div class="section-headline">
            <i class="section-headline__icon material-icons">work</i>{{ lang.experience }}
          </div>

          <div class="section-content">
            <div
              v-for="(experience, index) in person.experience"
              :key="index"
              class="section-content__item">

              <span class="section-content__header">
                {{ experience.position }}
                <span class="section-content__text" style="margin-right: -10px; margin-top: 3px;" v-if="experience.timeperiod.split('-').length === 2">&nbsp;-</span>
                <span class="section-content__text" style="margin-top: 3px;">{{ experience.timeperiod.split('-')[0] }}</span>
              </span>

              <span class="section-content__text" style="margin-top: 3px;">{{ experience.timeperiod.split('-')[1] }}</span>
              <span class="section-content__subheader">{{ experience.company }}</span>
              <div class="section-content__text--light">{{ experience.description }}</div>
              <hr v-if="index !== (person.experience.length - 1)" class="section-content__hr"/>
            </div>
          </div>
        </div>

        <div class="section">
          <div class="section-headline">
            <i class="section-headline__icon material-icons">school</i>{{ lang.education }}
          </div>

          <div class="section-content">
            <div
              v-for="(education, index) in person.education"
              class="section-content__item"
              :key="index">

              <span class="section-content__header">{{ education.position }}</span>
              <span class="section-content__subheader">
                {{ education.degree }}
                <span class="section-content__text" style="margin-right: -10px" v-if="education.timeperiod.split('-').length === 2">&nbsp;-</span>
                <span class="section-content__text">{{ education.timeperiod.split('-')[0] }}</span>
              </span>

              <div class="section-content__text">{{ education.timeperiod.split('-')[1] }}</div>
              <div class="section-content__text--light">{{ education.description }}</div>
              <hr v-if="index !== (person.education.length - 1)" class="section-content__hr"/>
            </div>
          </div>
        </div>

        <div
          v-if="person.projects"
          class="section">
          <div class="section-headline">
            <i class="section-headline__icon material-icons">code</i>{{ lang.projects }}
          </div>

          <div class="section-content">
            <a v-for="(project, index) in person.projects" :key="index"
               class="section-content__item"
               :href="project.url">
              <span class="section-content__text" style="margin-top: 2px">{{ project.year }}</span>
              <span class="section-content__header"> {{ project.name }} </span>
              <span v-if="project.platform" class="section-content__text--light" style="font-size: 0.875em">{{ project.platform }}</span>
              <div class="section-content__text--light" style="margin-top: 2px"> {{ project.description }} </div>
              <hr v-if="index !== (person.projects.length - 1)" class="section-content__hr"/>
            </a>
          </div>
        </div>

        <div
          v-if="person.hobbies"
          class="section">
          <div class="section-headline">
            <i class="section-headline__icon material-icons">schedule</i>{{ lang.hobbies }}
          </div>

          <div class="section-content-grid">
            <div v-for="(hobby, index) in person.hobbies"
              class="grid-item"
              :key="index">
              <span class="squared-grid-item">
                {{ hobby.name }}
              </span>
            </div>
          </div>
        </div>

      </div>
    </div>

    <img class="picture"/>
  </div>
</template>

<script>
  import Vue from 'vue';
  import { getVueOptions } from './options';

  const name = 'b1nd-v1';

  export default Vue.component(name, getVueOptions(name));
</script>

<style lang="less" scoped>
  @accent-color: #34495E;
  @banner-color: #42b883;
  @banner-height: 85px;
  @picture-size: 120px;
  @picture-offset: 35px;
  @base-padding: 20px;
  @left-column-width: 240px;

  a {
    color: inherit;
    cursor: pointer;
    text-decoration-line: none;

    &:visited {
      color: inherit;
    }
  }

  .resume {
    position: relative;
    font-family:'Roboto' !important;
    font-size: 0.9em;
  }

  .picture {
    display: none;
    position: absolute;
    top: @banner-height - @picture-offset;
    left: @left-column-width + @base-padding * 2 - @picture-size / 2;
    height: @picture-size;
    width: @picture-size;
    border-radius: 50%;
    border: 5px solid @accent-color;
    content: url('../../resume/id.jpg');
    z-index: 2;
  }

  .banner {
    width: calc(100% - @base-padding * 2);
    height: @banner-height;
    padding: @base-padding;
    background-color: @banner-color;
    /*
      background-image: url('../../resume/banner.png');
      background-repeat: no-repeat;
      background-size: cover;
    */
    color: white;

    &__fullname {
      font-size: 40px;
    }

    &__position {
      font-size: 25px;
    }

    &__location {
      font-size: 12px;
    }
  }

  .content {
    display: flex;
    width: 100%;
    height: 100%;

    &__left,
    &__right {
      height: 100%;
      background-color: white;
      padding: 10px @base-padding @base-padding;
    }

    &__left {
      width: @left-column-width;
      color: rgba(255, 255, 255, 0.59);
      background-color: @accent-color;

      .section-headline {
        color: white;
      }
    }

    &__right {
      flex: 1;
    }
  }

  .skill {
    clear:both;
    width:97%;
    padding-top:4px;
    .left {
      float:left;
      width:10%;
      padding-top:3px;
      i:nth-child(2) {
        float:left;
        padding-top:4px;
      }
    }
    .right {
      float:right;
      width:93%;
      .progress {
        float:left;
        position:relative;
        height:2px;
        display:block;
        width:95%;
        background-color:rgba(255,255,255,0.19);
        border-radius:2px;
        margin:0.5rem 0 1rem;
        overflow:visible;
        margin-bottom:10px;
        .determinate {
          background-color:#42b883;
          position:absolute;
          top:0;
          bottom:0;
          .fa, .material-icons {
            font-size:13px;
            position:absolute;
            top:-4px;
            right:-2px;
            margin-left:50%;
            color:white;
          }
        }
      }
    }
  }

  .section-item {
    margin-left: 20px;
  }

  .section-item-right {
    float:right;
    color:#42b883;
    margin-right: 20px;
  }

  .section {
    margin: 20px 0;
  }

  .section-link,
  .section-headline {
    display: flex !important;
    align-items: center;
    color: @accent-color;
    display: inline-block;
    font-size: 1.2em;
    margin: 8px 0;

    &__icon {
      margin-right: 8px;
      font-size: 1.4em;
    }
  }

  .section-link {
    font-size: 1.1em;
    color: rgba(255, 255, 255, 0.59) !important;

    &__icon {
      color: white;
    }
  }

  .section-content {
    display: block;
    margin-top: 5px;
    padding-left: 32px;
    font-size: 14px;

    &__item {
      display: block;
      margin-bottom: 10px;
    }

    &__hr {
      display: block;
      height: 1px;
      border: 0;
      border-top: 1px solid #ccc;
    }

    &__header {
      display: block;
      font-size: 1.1em;
      font-weight: 500;
    }

    &__subheader {
      display: block;
      font-size: 1.1em;
      font-weight: 400;
    }

    &__plain,
    &__text {
      font-size: 13px;
      font-weight: bold;
      float: right;

      &--light {
        margin-right: 55px;
        word-break: break-word;
        font-size: 13px;
      }
    }

    &__plain {
      display: block;
      font-weight: 300;
    }

    &__item-grid {
      flex: 1 1 0;
      margin-bottom: 5px;
      padding-right: 5px;
    }

    &--plain {
      padding: 0;
    }
  }

  .section-content-grid {
    display: flex;
    flex-wrap: wrap;
    margin-top: 5px;
    margin-bottom: 5px;
    padding-left: 32px;
  }

  .grid-item {
    padding-right: 5px;
  }

  .squared-grid-item {
    display: block;
    border: 1px solid black;
    color: black;
    margin-top: 5px;
    padding: 5px;
  }
</style>
